---
title: "Oktatóanyag: Azure Active Directory-integráció a szerződések ASC |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és az ASC érték szerződések között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: f7f54202-1581-4e55-a97e-02633ff9382d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/21/2017
ms.author: jeedes
ms.openlocfilehash: 8320af8acfda3e3d37e589c9887cd697d5ab651c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-asc-contracts"></a><span data-ttu-id="269f0-103">Oktatóanyag: Azure Active Directoryval integrált ASC szerződések</span><span class="sxs-lookup"><span data-stu-id="269f0-103">Tutorial: Azure Active Directory integration with ASC Contracts</span></span>

<span data-ttu-id="269f0-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate ASC szerződések az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="269f0-104">In this tutorial, you learn how toointegrate ASC Contracts with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="269f0-105">ASC szerződések integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="269f0-105">Integrating ASC Contracts with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="269f0-106">Szabályozhatja, aki hozzáféréssel rendelkezik az Azure AD-ben tooASC szerződések</span><span class="sxs-lookup"><span data-stu-id="269f0-106">You can control in Azure AD who has access tooASC Contracts</span></span>
- <span data-ttu-id="269f0-107">Engedélyezheti a felhasználók tooautomatically bejelentkezett tooASC szerződéseket (egyszeri bejelentkezés) az Azure AD-fiókok lekérése</span><span class="sxs-lookup"><span data-stu-id="269f0-107">You can enable your users tooautomatically get signed-on tooASC Contracts (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="269f0-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="269f0-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="269f0-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="269f0-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="269f0-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="269f0-110">Prerequisites</span></span>

<span data-ttu-id="269f0-111">az Azure AD integrálása ASC szerződések tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="269f0-111">tooconfigure Azure AD integration with ASC Contracts, you need hello following items:</span></span>

- <span data-ttu-id="269f0-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="269f0-112">An Azure AD subscription</span></span>
- <span data-ttu-id="269f0-113">Egy ASC szerződések egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="269f0-113">An ASC Contracts single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="269f0-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="269f0-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="269f0-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="269f0-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="269f0-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="269f0-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="269f0-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="269f0-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="269f0-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="269f0-118">Scenario description</span></span>
<span data-ttu-id="269f0-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="269f0-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="269f0-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="269f0-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="269f0-121">Hello gyűjteményből ASC szerződések hozzáadása</span><span class="sxs-lookup"><span data-stu-id="269f0-121">Adding ASC Contracts from hello gallery</span></span>
2. <span data-ttu-id="269f0-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="269f0-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-asc-contracts-from-hello-gallery"></a><span data-ttu-id="269f0-123">Hello gyűjteményből ASC szerződések hozzáadása</span><span class="sxs-lookup"><span data-stu-id="269f0-123">Adding ASC Contracts from hello gallery</span></span>
<span data-ttu-id="269f0-124">tooconfigure hello integrációs ASC szerződések az Azure AD-be, meg kell tooadd ASC szerződések hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="269f0-124">tooconfigure hello integration of ASC Contracts into Azure AD, you need tooadd ASC Contracts from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="269f0-125">**tooadd ASC szerződések hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="269f0-125">**tooadd ASC Contracts from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="269f0-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="269f0-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="269f0-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="269f0-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="269f0-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="269f0-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="269f0-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="269f0-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="269f0-133">Hello keresési mezőbe, írja be a **ASC szerződések**.</span><span class="sxs-lookup"><span data-stu-id="269f0-133">In hello search box, type **ASC Contracts**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-asccontracts-tutorial/tutorial_asccontracts_search.png)

5. <span data-ttu-id="269f0-135">A hello eredmények panelen válassza ki a **ASC szerződések**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="269f0-135">In hello results panel, select **ASC Contracts**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-asccontracts-tutorial/tutorial_asccontracts_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="269f0-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="269f0-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="269f0-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezést a ASC szerződések "Britta Simon." nevű tesztfelhasználó alapján</span><span class="sxs-lookup"><span data-stu-id="269f0-138">In this section, you configure and test Azure AD single sign-on with ASC Contracts based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="269f0-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói ASC szerződésekben tooa felhasználói az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="269f0-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in ASC Contracts is tooa user in Azure AD.</span></span> <span data-ttu-id="269f0-140">Ez azt jelenti hello kapcsolódó felhasználói ASC szerződések és az Azure AD-felhasználó közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="269f0-140">In other words, a link relationship between an Azure AD user and hello related user in ASC Contracts needs toobe established.</span></span>

<span data-ttu-id="269f0-141">Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** ASC szerződésekben.</span><span class="sxs-lookup"><span data-stu-id="269f0-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in ASC Contracts.</span></span>

<span data-ttu-id="269f0-142">tooconfigure és tesztelése az Azure AD egyszeri bejelentkezést a ASC szerződések, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="269f0-142">tooconfigure and test Azure AD single sign-on with ASC Contracts, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="269f0-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="269f0-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="269f0-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="269f0-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="269f0-145">**[Egy ASC szerződések tesztfelhasználó létrehozása](#creating-an-asc-contracts-test-user)**  -toohave egy megfelelője a Britta Simon ASC szerződésekben csatolt toohello az Azure AD felhasználói ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="269f0-145">**[Creating an ASC Contracts test user](#creating-an-asc-contracts-test-user)** - toohave a counterpart of Britta Simon in ASC Contracts that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="269f0-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="269f0-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="269f0-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="269f0-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="269f0-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="269f0-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="269f0-149">Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése, és az ASC szerződések alkalmazásban egyszeri bejelentkezés beállítása.</span><span class="sxs-lookup"><span data-stu-id="269f0-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your ASC Contracts application.</span></span>

<span data-ttu-id="269f0-150">**az Azure AD tooconfigure egyszeri bejelentkezést a ASC szerződések, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="269f0-150">**tooconfigure Azure AD single sign-on with ASC Contracts, perform hello following steps:**</span></span>

1. <span data-ttu-id="269f0-151">Az Azure portál, a hello hello **ASC szerződések** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="269f0-151">In hello Azure portal, on hello **ASC Contracts** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="269f0-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="269f0-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-asccontracts-tutorial/tutorial_asccontracts_samlbase.png)

3. <span data-ttu-id="269f0-155">A hello **ASC szerződések tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="269f0-155">On hello **ASC Contracts Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-asccontracts-tutorial/tutorial_asccontracts_url.png)

    <span data-ttu-id="269f0-157">a.</span><span class="sxs-lookup"><span data-stu-id="269f0-157">a.</span></span> <span data-ttu-id="269f0-158">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<subdomain>.asccontracts.com/shibboleth`</span><span class="sxs-lookup"><span data-stu-id="269f0-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<subdomain>.asccontracts.com/shibboleth`</span></span>

    <span data-ttu-id="269f0-159">b.</span><span class="sxs-lookup"><span data-stu-id="269f0-159">b.</span></span> <span data-ttu-id="269f0-160">A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<subdomain>.asccontracts.com/shibboleth.sso/login`</span><span class="sxs-lookup"><span data-stu-id="269f0-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<subdomain>.asccontracts.com/shibboleth.sso/login`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="269f0-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="269f0-161">These values are not real.</span></span> <span data-ttu-id="269f0-162">Frissítheti ezeket az értékeket hello tényleges azonosítója és a válasz URL-címmel.</span><span class="sxs-lookup"><span data-stu-id="269f0-162">Update these values with hello actual Identifier and Reply URL.</span></span> <span data-ttu-id="269f0-163">Lépjen kapcsolatba a ASC hálózatok Inc. (ASC) csapatának **613.599.6178** tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="269f0-163">Contact ASC Networks Inc. (ASC) team at **613.599.6178** tooget these values.</span></span>

4. <span data-ttu-id="269f0-164">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="269f0-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-asccontracts-tutorial/tutorial_asccontracts_certificate.png) 

5. <span data-ttu-id="269f0-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="269f0-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-asccontracts-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="269f0-168">tooconfigure egyszeri bejelentkezést a **ASC szerződések** oldalán, ASC hálózatok Inc. (ASC) támogatását hívás **613.599.6178** és adja meg a letöltött hello **metaadatainak XML-kódja**.</span><span class="sxs-lookup"><span data-stu-id="269f0-168">tooconfigure single sign-on on **ASC Contracts** side, call ASC Networks Inc. (ASC) support at **613.599.6178** and provide them with hello downloaded **Metadata XML**.</span></span> <span data-ttu-id="269f0-169">Maguk állítják be a toohave hello SAML SSO kapcsolat mindkét oldalán megfelelően beállítva fel ezt az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="269f0-169">They set this application up toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="269f0-170">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="269f0-170">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="269f0-171">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="269f0-171">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="269f0-172">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="269f0-172">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="269f0-173">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="269f0-173">Creating an Azure AD test user</span></span>
<span data-ttu-id="269f0-174">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="269f0-174">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="269f0-176">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="269f0-176">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="269f0-177">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="269f0-177">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-asccontracts-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="269f0-179">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="269f0-179">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-asccontracts-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="269f0-181">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="269f0-181">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-asccontracts-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="269f0-183">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="269f0-183">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-asccontracts-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="269f0-185">a.</span><span class="sxs-lookup"><span data-stu-id="269f0-185">a.</span></span> <span data-ttu-id="269f0-186">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="269f0-186">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="269f0-187">b.</span><span class="sxs-lookup"><span data-stu-id="269f0-187">b.</span></span> <span data-ttu-id="269f0-188">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="269f0-188">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="269f0-189">c.</span><span class="sxs-lookup"><span data-stu-id="269f0-189">c.</span></span> <span data-ttu-id="269f0-190">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="269f0-190">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="269f0-191">d.</span><span class="sxs-lookup"><span data-stu-id="269f0-191">d.</span></span> <span data-ttu-id="269f0-192">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="269f0-192">Click **Create**.</span></span>
 
### <a name="creating-an-asc-contracts-test-user"></a><span data-ttu-id="269f0-193">Egy ASC szerződések tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="269f0-193">Creating an ASC Contracts test user</span></span>

<span data-ttu-id="269f0-194">ASC hálózatok Inc. (ASC) támogatási csapatának együttműködve **613.599.6178** tooget hello felhasználók hello ASC szerződések platform szerepel.</span><span class="sxs-lookup"><span data-stu-id="269f0-194">Work with ASC Networks Inc. (ASC) support team at **613.599.6178** tooget hello users added in hello ASC Contracts platform.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="269f0-195">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="269f0-195">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="269f0-196">Ebben a szakaszban engedélyezése az Azure egyszeri bejelentkezés Britta Simon toouse tooASC szerződések hozzáférés biztosítása.</span><span class="sxs-lookup"><span data-stu-id="269f0-196">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooASC Contracts.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="269f0-198">**tooassign Britta Simon tooASC szerződések, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="269f0-198">**tooassign Britta Simon tooASC Contracts, perform hello following steps:**</span></span>

1. <span data-ttu-id="269f0-199">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="269f0-199">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="269f0-201">Hello alkalmazások listában válassza ki a **ASC szerződések**.</span><span class="sxs-lookup"><span data-stu-id="269f0-201">In hello applications list, select **ASC Contracts**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-asccontracts-tutorial/tutorial_asccontracts_app.png) 

3. <span data-ttu-id="269f0-203">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="269f0-203">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="269f0-205">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="269f0-205">Click **Add** button.</span></span> <span data-ttu-id="269f0-206">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="269f0-206">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="269f0-208">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="269f0-208">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="269f0-209">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="269f0-209">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="269f0-210">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="269f0-210">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="269f0-211">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="269f0-211">Testing single sign-on</span></span>

<span data-ttu-id="269f0-212">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="269f0-212">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="269f0-213">Hello ASC szerződések hello hozzáférési Panel csempére kattintva kapja meg automatikusan bejelentkezett tooyour ASC szerződések alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="269f0-213">When you click hello ASC Contracts tile in hello Access Panel, you should get automatically signed-on tooyour ASC Contracts application.</span></span> <span data-ttu-id="269f0-214">A hozzáférési Panel kapcsolatos további információkért tekintse meg.</span><span class="sxs-lookup"><span data-stu-id="269f0-214">For more information about the Access Panel, see.</span></span> <span data-ttu-id="269f0-215">[A hozzáférési Panel bemutatása](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="269f0-215">[Introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="269f0-216">További források</span><span class="sxs-lookup"><span data-stu-id="269f0-216">Additional resources</span></span>

* [<span data-ttu-id="269f0-217">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="269f0-217">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="269f0-218">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="269f0-218">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-asccontracts-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-asccontracts-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-asccontracts-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-asccontracts-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-asccontracts-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-asccontracts-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-asccontracts-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-asccontracts-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-asccontracts-tutorial/tutorial_general_203.png

