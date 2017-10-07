---
title: "Oktatóanyag: Azure Active Directoryval integrált FM:Systems |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és FM:Systems között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: f78c58c5-6e98-458b-8991-78624a245665
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/17/2017
ms.author: jeedes
ms.openlocfilehash: 677ef74dac663a43835d65a4d4f4fd031a0078cd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-fmsystems"></a><span data-ttu-id="5c7df-103">Oktatóanyag: Azure Active Directoryval integrált FM:Systems</span><span class="sxs-lookup"><span data-stu-id="5c7df-103">Tutorial: Azure Active Directory integration with FM:Systems</span></span>

<span data-ttu-id="5c7df-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate FM:Systems az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="5c7df-104">In this tutorial, you learn how toointegrate FM:Systems with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="5c7df-105">FM:Systems integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="5c7df-105">Integrating FM:Systems with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="5c7df-106">Megadhatja a hozzáférés tooFM:Systems rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="5c7df-106">You can control in Azure AD who has access tooFM:Systems</span></span>
- <span data-ttu-id="5c7df-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooFM:Systems (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="5c7df-107">You can enable your users tooautomatically get signed-on tooFM:Systems (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="5c7df-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="5c7df-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="5c7df-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="5c7df-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5c7df-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="5c7df-110">Prerequisites</span></span>

<span data-ttu-id="5c7df-111">az Azure AD integrálása FM:Systems tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="5c7df-111">tooconfigure Azure AD integration with FM:Systems, you need hello following items:</span></span>

- <span data-ttu-id="5c7df-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="5c7df-112">An Azure AD subscription</span></span>
- <span data-ttu-id="5c7df-113">Egy FM:Systems egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="5c7df-113">An FM:Systems single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="5c7df-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="5c7df-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="5c7df-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="5c7df-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="5c7df-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="5c7df-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="5c7df-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="5c7df-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="5c7df-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="5c7df-118">Scenario description</span></span>
<span data-ttu-id="5c7df-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="5c7df-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="5c7df-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="5c7df-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="5c7df-121">Hello gyűjteményből FM:Systems hozzáadása</span><span class="sxs-lookup"><span data-stu-id="5c7df-121">Adding FM:Systems from hello gallery</span></span>
2. <span data-ttu-id="5c7df-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="5c7df-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-fmsystems-from-hello-gallery"></a><span data-ttu-id="5c7df-123">Hello gyűjteményből FM:Systems hozzáadása</span><span class="sxs-lookup"><span data-stu-id="5c7df-123">Adding FM:Systems from hello gallery</span></span>
<span data-ttu-id="5c7df-124">tooconfigure hello integrációja FM:Systems az Azure AD-be, meg kell tooadd FM:Systems hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="5c7df-124">tooconfigure hello integration of FM:Systems into Azure AD, you need tooadd FM:Systems from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="5c7df-125">**tooadd FM:Systems hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="5c7df-125">**tooadd FM:Systems from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="5c7df-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="5c7df-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="5c7df-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="5c7df-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="5c7df-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="5c7df-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="5c7df-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="5c7df-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="5c7df-133">Hello keresési mezőbe, írja be a **FM:Systems**.</span><span class="sxs-lookup"><span data-stu-id="5c7df-133">In hello search box, type **FM:Systems**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-fm-systems-tutorial/tutorial_fmsystems_search.png)

5. <span data-ttu-id="5c7df-135">A hello eredmények panelen válassza ki a **FM:Systems**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="5c7df-135">In hello results panel, select **FM:Systems**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-fm-systems-tutorial/tutorial_fmsystems_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="5c7df-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="5c7df-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="5c7df-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján FM:Systems.</span><span class="sxs-lookup"><span data-stu-id="5c7df-138">In this section, you configure and test Azure AD single sign-on with FM:Systems based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="5c7df-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó FM:Systems tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="5c7df-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in FM:Systems is tooa user in Azure AD.</span></span> <span data-ttu-id="5c7df-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello FM:Systems közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="5c7df-140">In other words, a link relationship between an Azure AD user and hello related user in FM:Systems needs toobe established.</span></span>

<span data-ttu-id="5c7df-141">FM:Systems, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="5c7df-141">In FM:Systems, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="5c7df-142">tooconfigure és az Azure AD az egyszeri bejelentkezés FM:Systems-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="5c7df-142">tooconfigure and test Azure AD single sign-on with FM:Systems, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="5c7df-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="5c7df-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="5c7df-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="5c7df-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="5c7df-145">**[Egy FM:Systems tesztfelhasználó létrehozása](#creating-an-fmsystems-test-user)**  -toohave Britta Simon egy partner, a FM:Systems, amely a felhasználó csatolt toohello az Azure AD-ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="5c7df-145">**[Creating an FM:Systems test user](#creating-an-fmsystems-test-user)** - toohave a counterpart of Britta Simon in FM:Systems that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="5c7df-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="5c7df-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="5c7df-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="5c7df-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="5c7df-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="5c7df-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="5c7df-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az FM:Systems alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="5c7df-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your FM:Systems application.</span></span>

<span data-ttu-id="5c7df-150">**az Azure AD tooconfigure egyszeri bejelentkezést a FM:Systems, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="5c7df-150">**tooconfigure Azure AD single sign-on with FM:Systems, perform hello following steps:**</span></span>

1. <span data-ttu-id="5c7df-151">Az Azure portál, a hello hello **FM:Systems** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="5c7df-151">In hello Azure portal, on hello **FM:Systems** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="5c7df-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="5c7df-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-fm-systems-tutorial/tutorial_fmsystems_samlbase.png)

3. <span data-ttu-id="5c7df-155">A hello **FM:Systems tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="5c7df-155">On hello **FM:Systems Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-fm-systems-tutorial/tutorial_fmsystems_url.png)

    <span data-ttu-id="5c7df-157">A hello **válasz URL-CÍMEN** szövegmező, írja be a FM:Systems **válasz URL-CÍMEN**, típus hello URL-cím a következő mintát hello használata:`https://<companyname>.fmshosted.com/fminteract/ConsumerService2.aspx`</span><span class="sxs-lookup"><span data-stu-id="5c7df-157">In hello **Reply URL** textbox, type your FM:Systems **Reply URL**, type hello URL using hello following pattern: `https://<companyname>.fmshosted.com/fminteract/ConsumerService2.aspx`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="5c7df-158">Ez az érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="5c7df-158">This value is not real.</span></span> <span data-ttu-id="5c7df-159">Frissítse ezt az értéket a hello tényleges válasz URL-CÍMEN.</span><span class="sxs-lookup"><span data-stu-id="5c7df-159">Update this value with hello actual Reply URL.</span></span> <span data-ttu-id="5c7df-160">Ügyfél [FM:Systems támogatási csoport](https://fmsystems.com/ask-us/) tooget ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="5c7df-160">Contact [FM:Systems support team](https://fmsystems.com/ask-us/) tooget this value.</span></span>
 
4. <span data-ttu-id="5c7df-161">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="5c7df-161">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-fm-systems-tutorial/tutorial_fmsystems_certificate.png) 

5. <span data-ttu-id="5c7df-163">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="5c7df-163">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-fm-systems-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="5c7df-165">tooconfigure egyszeri bejelentkezést a **FM:Systems** oldalon kell letöltött toosend hello **metaadatainak XML-kódja** túl[FM:Systems támogatási csoport](https://fmsystems.com/ask-us/).</span><span class="sxs-lookup"><span data-stu-id="5c7df-165">tooconfigure single sign-on on **FM:Systems** side, you need toosend hello downloaded **Metadata XML** too[FM:Systems support team](https://fmsystems.com/ask-us/).</span></span> <span data-ttu-id="5c7df-166">Maguk állítják be ezt a beállítást toohave hello SAML SSO kapcsolat mindkét oldalán megfelelően beállítva.</span><span class="sxs-lookup"><span data-stu-id="5c7df-166">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span> <span data-ttu-id="5c7df-167">Ha egyszeri bejelentkezés engedélyezve van az előfizetés értesítést kap.</span><span class="sxs-lookup"><span data-stu-id="5c7df-167">You will get a notification when SSO has been enabled for your subscription.</span></span>

> [!TIP]
> <span data-ttu-id="5c7df-168">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="5c7df-168">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="5c7df-169">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="5c7df-169">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="5c7df-170">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="5c7df-170">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="5c7df-171">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="5c7df-171">Creating an Azure AD test user</span></span>
<span data-ttu-id="5c7df-172">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="5c7df-172">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="5c7df-174">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="5c7df-174">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="5c7df-175">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="5c7df-175">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-fm-systems-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="5c7df-177">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="5c7df-177">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-fm-systems-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="5c7df-179">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="5c7df-179">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-fm-systems-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="5c7df-181">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="5c7df-181">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-fm-systems-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="5c7df-183">a.</span><span class="sxs-lookup"><span data-stu-id="5c7df-183">a.</span></span> <span data-ttu-id="5c7df-184">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="5c7df-184">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="5c7df-185">b.</span><span class="sxs-lookup"><span data-stu-id="5c7df-185">b.</span></span> <span data-ttu-id="5c7df-186">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="5c7df-186">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="5c7df-187">c.</span><span class="sxs-lookup"><span data-stu-id="5c7df-187">c.</span></span> <span data-ttu-id="5c7df-188">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="5c7df-188">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="5c7df-189">d.</span><span class="sxs-lookup"><span data-stu-id="5c7df-189">d.</span></span> <span data-ttu-id="5c7df-190">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="5c7df-190">Click **Create**.</span></span>
 
### <a name="creating-an-fmsystems-test-user"></a><span data-ttu-id="5c7df-191">Egy FM:Systems tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="5c7df-191">Creating an FM:Systems test user</span></span>

1. <span data-ttu-id="5c7df-192">A webböngésző ablakának jelentkezzen be a FM:Systems vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="5c7df-192">In a web browser window, log into your FM:Systems company site as an administrator.</span></span>

2. <span data-ttu-id="5c7df-193">Nyissa meg túl**rendszerfelügyelet \> kezelése biztonsági \> felhasználók \> Felhasználólista**.</span><span class="sxs-lookup"><span data-stu-id="5c7df-193">Go too**System Administration \> Manage Security \> Users \> User list**.</span></span>
   
    <span data-ttu-id="5c7df-194">![Rendszer-felügyeleti](./media/active-directory-saas-fm-systems-tutorial/ic795905.png "rendszerfelügyelet")</span><span class="sxs-lookup"><span data-stu-id="5c7df-194">![System Administration](./media/active-directory-saas-fm-systems-tutorial/ic795905.png "System Administration")</span></span>

3. <span data-ttu-id="5c7df-195">Kattintson a **új felhasználó létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="5c7df-195">Click **Create new user**.</span></span>
   
    <span data-ttu-id="5c7df-196">![Új felhasználó létrehozása](./media/active-directory-saas-fm-systems-tutorial/ic795906.png "új felhasználó létrehozása")</span><span class="sxs-lookup"><span data-stu-id="5c7df-196">![Create New User](./media/active-directory-saas-fm-systems-tutorial/ic795906.png "Create New User")</span></span>

4. <span data-ttu-id="5c7df-197">A hello **felhasználó létrehozása** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="5c7df-197">In hello **Create User** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="5c7df-198">![Hozzon létre felhasználói](./media/active-directory-saas-fm-systems-tutorial/ic795907.png "felhasználó létrehozása")</span><span class="sxs-lookup"><span data-stu-id="5c7df-198">![Create User](./media/active-directory-saas-fm-systems-tutorial/ic795907.png "Create User")</span></span>
   
    <span data-ttu-id="5c7df-199">a.</span><span class="sxs-lookup"><span data-stu-id="5c7df-199">a.</span></span> <span data-ttu-id="5c7df-200">Típus hello **felhasználónév**, hello **jelszó**, **jelszó megerősítése**, **E-mail** és hello **Alkalmazottazonosító**egy érvényes Azure Active Directory-fiókot a kívánt tooprovision hello kapcsolódó szövegmezőből.</span><span class="sxs-lookup"><span data-stu-id="5c7df-200">Type hello **UserName**, hello **Password**, **Confirm Password**, **E-mail** and hello **Employee ID** of a valid Azure Active Directory account you want tooprovision into hello related textboxes.</span></span>
   
    <span data-ttu-id="5c7df-201">b.</span><span class="sxs-lookup"><span data-stu-id="5c7df-201">b.</span></span> <span data-ttu-id="5c7df-202">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="5c7df-202">Click **Next**.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="5c7df-203">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="5c7df-203">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="5c7df-204">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooFM:Systems megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="5c7df-204">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooFM:Systems.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="5c7df-206">**tooassign Britta Simon tooFM:Systems, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="5c7df-206">**tooassign Britta Simon tooFM:Systems, perform hello following steps:**</span></span>

1. <span data-ttu-id="5c7df-207">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="5c7df-207">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="5c7df-209">Hello alkalmazások listában válassza ki a **FM:Systems**.</span><span class="sxs-lookup"><span data-stu-id="5c7df-209">In hello applications list, select **FM:Systems**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-fm-systems-tutorial/tutorial_fmsystems_app.png) 

3. <span data-ttu-id="5c7df-211">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="5c7df-211">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="5c7df-213">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="5c7df-213">Click **Add** button.</span></span> <span data-ttu-id="5c7df-214">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="5c7df-214">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="5c7df-216">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="5c7df-216">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="5c7df-217">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="5c7df-217">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="5c7df-218">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="5c7df-218">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="5c7df-219">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="5c7df-219">Testing single sign-on</span></span>

<span data-ttu-id="5c7df-220">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="5c7df-220">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="5c7df-221">Ha a hozzáférési Panel hello hello FM:Systems csempe gombra kattint, automatikusan bejelentkezett tooyour FM:Systems alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="5c7df-221">When you click hello FM:Systems tile in hello Access Panel, you should get automatically signed-on tooyour FM:Systems application.</span></span>
<span data-ttu-id="5c7df-222">A hozzáférési Panel kapcsolatos további információkért lásd: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="5c7df-222">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5c7df-223">További források</span><span class="sxs-lookup"><span data-stu-id="5c7df-223">Additional resources</span></span>

* [<span data-ttu-id="5c7df-224">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="5c7df-224">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="5c7df-225">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="5c7df-225">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-fm-systems-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-fm-systems-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-fm-systems-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-fm-systems-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-fm-systems-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-fm-systems-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-fm-systems-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-fm-systems-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-fm-systems-tutorial/tutorial_general_203.png

