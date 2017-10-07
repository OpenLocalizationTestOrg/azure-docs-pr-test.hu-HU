---
title: "Oktatóanyag: Azure Active Directoryval integrált által Desire2Learn Brightspace |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Brightspace által Desire2Learn között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: e2d3065b-1f6c-4c45-af78-0d5da3266999
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: 99d03dc50defcb291a651a5415e30baab39e1e77
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-brightspace-by-desire2learn"></a><span data-ttu-id="703fa-103">Oktatóanyag: Azure Active Directoryval integrált Brightspace Desire2Learn által</span><span class="sxs-lookup"><span data-stu-id="703fa-103">Tutorial: Azure Active Directory integration with Brightspace by Desire2Learn</span></span>

<span data-ttu-id="703fa-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Brightspace által Desire2Learn az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="703fa-104">In this tutorial, you learn how toointegrate Brightspace by Desire2Learn with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="703fa-105">Brightspace által Desire2Learn integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="703fa-105">Integrating Brightspace by Desire2Learn with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="703fa-106">Megadhatja a hozzáférés tooBrightspace által Desire2Learn rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="703fa-106">You can control in Azure AD who has access tooBrightspace by Desire2Learn</span></span>
- <span data-ttu-id="703fa-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooBrightspace által Desire2Learn (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="703fa-107">You can enable your users tooautomatically get signed-on tooBrightspace by Desire2Learn (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="703fa-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="703fa-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="703fa-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="703fa-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="703fa-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="703fa-110">Prerequisites</span></span>

<span data-ttu-id="703fa-111">tooconfigure Brightspace Desire2Learn által az Azure AD integrálása, a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="703fa-111">tooconfigure Azure AD integration with Brightspace by Desire2Learn, you need hello following items:</span></span>

- <span data-ttu-id="703fa-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="703fa-112">An Azure AD subscription</span></span>
- <span data-ttu-id="703fa-113">Egy Brightspace Desire2Learn egyszeri bejelentkezés által engedélyezett előfizetés</span><span class="sxs-lookup"><span data-stu-id="703fa-113">A Brightspace by Desire2Learn single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="703fa-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="703fa-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="703fa-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="703fa-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="703fa-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="703fa-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="703fa-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="703fa-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="703fa-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="703fa-118">Scenario description</span></span>
<span data-ttu-id="703fa-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="703fa-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="703fa-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="703fa-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="703fa-121">Hello gyűjteményből által Desire2Learn Brightspace hozzáadása</span><span class="sxs-lookup"><span data-stu-id="703fa-121">Adding Brightspace by Desire2Learn from hello gallery</span></span>
2. <span data-ttu-id="703fa-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="703fa-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-brightspace-by-desire2learn-from-hello-gallery"></a><span data-ttu-id="703fa-123">Hello gyűjteményből által Desire2Learn Brightspace hozzáadása</span><span class="sxs-lookup"><span data-stu-id="703fa-123">Adding Brightspace by Desire2Learn from hello gallery</span></span>
<span data-ttu-id="703fa-124">tooconfigure hello integrációja Brightspace Desire2Learn által az Azure AD-be, meg kell tooadd által Desire2Learn Brightspace hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="703fa-124">tooconfigure hello integration of Brightspace by Desire2Learn into Azure AD, you need tooadd Brightspace by Desire2Learn from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="703fa-125">**tooadd Brightspace által Desire2Learn hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="703fa-125">**tooadd Brightspace by Desire2Learn from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="703fa-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="703fa-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="703fa-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="703fa-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="703fa-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="703fa-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="703fa-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="703fa-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="703fa-133">Hello keresési mezőbe, írja be a **által Desire2Learn Brightspace**.</span><span class="sxs-lookup"><span data-stu-id="703fa-133">In hello search box, type **Brightspace by Desire2Learn**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-brightspace-desire2learn-tutorial/tutorial_brightspacebydesire2learn_search.png)

5. <span data-ttu-id="703fa-135">A hello eredmények panelen válassza ki a **Brightspace Desire2Learn által**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="703fa-135">In hello results panel, select **Brightspace by Desire2Learn**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-brightspace-desire2learn-tutorial/tutorial_brightspacebydesire2learn_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="703fa-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="703fa-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="703fa-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezést a Brightspace "Britta Simon" nevű tesztfelhasználó alapján Desire2Learn által.</span><span class="sxs-lookup"><span data-stu-id="703fa-138">In this section, you configure and test Azure AD single sign-on with Brightspace by Desire2Learn based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="703fa-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó által Desire2Learn Brightspace tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="703fa-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Brightspace by Desire2Learn is tooa user in Azure AD.</span></span> <span data-ttu-id="703fa-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználói hello Brightspace Desire2Learn által a hivatkozás kapcsolatának kell toobe létrejött.</span><span class="sxs-lookup"><span data-stu-id="703fa-140">In other words, a link relationship between an Azure AD user and hello related user in Brightspace by Desire2Learn needs toobe established.</span></span>

<span data-ttu-id="703fa-141">Brightspace Desire2Learn által, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="703fa-141">In Brightspace by Desire2Learn, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="703fa-142">tooconfigure és az Azure AD az egyszeri bejelentkezés által Desire2Learn Brightspace-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="703fa-142">tooconfigure and test Azure AD single sign-on with Brightspace by Desire2Learn, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="703fa-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="703fa-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="703fa-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="703fa-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="703fa-145">**[Egy Brightspace Desire2Learn teszt felhasználó létrehozása](#creating-a-brightspace-by-desire2learn-test-user)**  -toohave egy Britta Simon az által, hogy a felhasználó csatolt toohello az Azure AD ábrázolása Desire2Learn Brightspace megfelelője.</span><span class="sxs-lookup"><span data-stu-id="703fa-145">**[Creating a Brightspace by Desire2Learn test user](#creating-a-brightspace-by-desire2learn-test-user)** - toohave a counterpart of Britta Simon in Brightspace by Desire2Learn that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="703fa-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="703fa-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="703fa-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="703fa-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="703fa-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="703fa-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="703fa-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez a Brightspace Desire2Learn alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="703fa-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Brightspace by Desire2Learn application.</span></span>

<span data-ttu-id="703fa-150">**az Azure AD tooconfigure egyszeri bejelentkezés Brightspace Desire2Learn, amelyet a hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="703fa-150">**tooconfigure Azure AD single sign-on with Brightspace by Desire2Learn, perform hello following steps:**</span></span>

1. <span data-ttu-id="703fa-151">Az Azure portál, a hello hello **által Desire2Learn Brightspace** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="703fa-151">In hello Azure portal, on hello **Brightspace by Desire2Learn** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="703fa-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="703fa-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-brightspace-desire2learn-tutorial/tutorial_brightspacebydesire2learn_samlbase.png)

3. <span data-ttu-id="703fa-155">A hello **Brightspace Desire2Learn tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="703fa-155">On hello **Brightspace by Desire2Learn Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-brightspace-desire2learn-tutorial/tutorial_brightspacebydesire2learn_url.png)

    <span data-ttu-id="703fa-157">a.</span><span class="sxs-lookup"><span data-stu-id="703fa-157">a.</span></span> <span data-ttu-id="703fa-158">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:</span><span class="sxs-lookup"><span data-stu-id="703fa-158">In hello **Identifier** textbox, type a URL using hello following pattern:</span></span>
    | |
    |--|
    | `https://<companyname>.tenants.brightspace.com/samlLogin`|
    | `https://<companyname>.desire2learn.com/shibboleth-sp`|

    <span data-ttu-id="703fa-159">b.</span><span class="sxs-lookup"><span data-stu-id="703fa-159">b.</span></span> <span data-ttu-id="703fa-160">A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<companyname>.desire2learn.com/d2l/lp/auth/login/samlLogin.d2l`</span><span class="sxs-lookup"><span data-stu-id="703fa-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<companyname>.desire2learn.com/d2l/lp/auth/login/samlLogin.d2l`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="703fa-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="703fa-161">These values are not real.</span></span> <span data-ttu-id="703fa-162">Frissítheti ezeket az értékeket hello tényleges azonosítója és a válasz URL-címmel.</span><span class="sxs-lookup"><span data-stu-id="703fa-162">Update these values with hello actual Identifier and Reply URL.</span></span> <span data-ttu-id="703fa-163">Ügyfél [Desire2Learn támogatási csapat Brightspace](https://www.d2l.com/contact/) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="703fa-163">Contact [Brightspace by Desire2Learn support team](https://www.d2l.com/contact/) tooget these values.</span></span>
 


4. <span data-ttu-id="703fa-164">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="703fa-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-brightspace-desire2learn-tutorial/tutorial_brightspacebydesire2learn_certificate.png) 

5. <span data-ttu-id="703fa-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="703fa-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-brightspace-desire2learn-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="703fa-168">tooconfigure egyszeri bejelentkezést a **által Desire2Learn Brightspace** oldalon kell letöltött toosend hello **metaadatainak XML-kódja** túl[Brightspace Desire2Learn támogatási csapat](https://www.d2l.com/contact/).</span><span class="sxs-lookup"><span data-stu-id="703fa-168">tooconfigure single sign-on on **Brightspace by Desire2Learn** side, you need toosend hello downloaded **Metadata XML** too[Brightspace by Desire2Learn support team](https://www.d2l.com/contact/).</span></span>

> [!TIP]
> <span data-ttu-id="703fa-169">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="703fa-169">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="703fa-170">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="703fa-170">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="703fa-171">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="703fa-171">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="703fa-172">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="703fa-172">Creating an Azure AD test user</span></span>
<span data-ttu-id="703fa-173">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="703fa-173">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="703fa-175">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="703fa-175">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="703fa-176">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="703fa-176">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-brightspace-desire2learn-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="703fa-178">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="703fa-178">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-brightspace-desire2learn-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="703fa-180">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="703fa-180">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-brightspace-desire2learn-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="703fa-182">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="703fa-182">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-brightspace-desire2learn-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="703fa-184">a.</span><span class="sxs-lookup"><span data-stu-id="703fa-184">a.</span></span> <span data-ttu-id="703fa-185">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="703fa-185">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="703fa-186">b.</span><span class="sxs-lookup"><span data-stu-id="703fa-186">b.</span></span> <span data-ttu-id="703fa-187">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="703fa-187">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="703fa-188">c.</span><span class="sxs-lookup"><span data-stu-id="703fa-188">c.</span></span> <span data-ttu-id="703fa-189">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="703fa-189">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="703fa-190">d.</span><span class="sxs-lookup"><span data-stu-id="703fa-190">d.</span></span> <span data-ttu-id="703fa-191">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="703fa-191">Click **Create**.</span></span>
 
### <a name="creating-a-brightspace-by-desire2learn-test-user"></a><span data-ttu-id="703fa-192">Egy Brightspace által Desire2Learn tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="703fa-192">Creating a Brightspace by Desire2Learn test user</span></span>

<span data-ttu-id="703fa-193">A sorrend tooenable az Azure AD felhasználók toolog Brightspace Desire2Learn által történő azok kell üzembe Brightspace történő Desire2Learn által.</span><span class="sxs-lookup"><span data-stu-id="703fa-193">In order tooenable Azure AD users toolog into Brightspace by Desire2Learn, they must be provisioned into Brightspace by Desire2Learn.</span></span>  

<span data-ttu-id="703fa-194">Brightspace által Desire2Learn hello esetben hello felhasználói fiókokat kell toobe hozta létre a [Desire2Learn támogatási csapat Brightspace](https://www.d2l.com/contact/).</span><span class="sxs-lookup"><span data-stu-id="703fa-194">In hello case of Brightspace by Desire2Learn, hello user accounts need toobe created by your [Brightspace by Desire2Learn support team](https://www.d2l.com/contact/).</span></span>

>[!NOTE]
><span data-ttu-id="703fa-195">Bármely más Brightspace Desire2Learn felhasználói fiók létrehozása eszközök is használhatja, vagy API-k által biztosított Brightspace Desire2Learn tooprovision Azure Active Directory felhasználói fiókokat.</span><span class="sxs-lookup"><span data-stu-id="703fa-195">You can use any other Brightspace by Desire2Learn user account creation tools or APIs provided by Brightspace by Desire2Learn tooprovision Azure Active Directory user accounts.</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="703fa-196">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="703fa-196">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="703fa-197">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooBrightspace által Desire2Learn megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="703fa-197">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooBrightspace by Desire2Learn.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="703fa-199">**tooassign Britta Simon tooBrightspace által Desire2Learn, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="703fa-199">**tooassign Britta Simon tooBrightspace by Desire2Learn, perform hello following steps:**</span></span>

1. <span data-ttu-id="703fa-200">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="703fa-200">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="703fa-202">Hello alkalmazások listában válassza ki a **által Desire2Learn Brightspace**.</span><span class="sxs-lookup"><span data-stu-id="703fa-202">In hello applications list, select **Brightspace by Desire2Learn**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-brightspace-desire2learn-tutorial/tutorial_brightspacebydesire2learn_app.png) 

3. <span data-ttu-id="703fa-204">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="703fa-204">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="703fa-206">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="703fa-206">Click **Add** button.</span></span> <span data-ttu-id="703fa-207">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="703fa-207">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="703fa-209">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="703fa-209">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="703fa-210">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="703fa-210">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="703fa-211">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="703fa-211">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="703fa-212">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="703fa-212">Testing single sign-on</span></span>

<span data-ttu-id="703fa-213">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="703fa-213">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="703fa-214">Hello Brightspace által Desire2Learn csempe a hozzáférési Panel hello kattintáskor automatikusan bejelentkezett tooyour Brightspace Desire2Learn alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="703fa-214">When you click hello Brightspace by Desire2Learn tile in hello Access Panel, you should get automatically signed-on tooyour Brightspace by Desire2Learn application.</span></span>
<span data-ttu-id="703fa-215">A hozzáférési Panel kapcsolatos további információkért lásd: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="703fa-215">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="703fa-216">További források</span><span class="sxs-lookup"><span data-stu-id="703fa-216">Additional resources</span></span>

* [<span data-ttu-id="703fa-217">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="703fa-217">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="703fa-218">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="703fa-218">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-brightspace-desire2learn-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-brightspace-desire2learn-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-brightspace-desire2learn-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-brightspace-desire2learn-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-brightspace-desire2learn-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-brightspace-desire2learn-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-brightspace-desire2learn-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-brightspace-desire2learn-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-brightspace-desire2learn-tutorial/tutorial_general_203.png

