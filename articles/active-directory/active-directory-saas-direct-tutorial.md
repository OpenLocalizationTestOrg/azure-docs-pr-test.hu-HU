---
title: "Oktatóanyag: Azure Active Directory-integráció közvetlen |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a közvetlen."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 7c2cd1f0-d14c-42f0-94a8-9b800008b285
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: ac663070b39e55eade2c43814b63a9d0374c7316
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-direct"></a><span data-ttu-id="583e9-103">Oktatóanyag: Azure Active Directory-integráció közvetlen</span><span class="sxs-lookup"><span data-stu-id="583e9-103">Tutorial: Azure Active Directory integration with Direct</span></span>

<span data-ttu-id="583e9-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate közvetlenül az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="583e9-104">In this tutorial, you learn how toointegrate Direct with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="583e9-105">Közvetlen integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="583e9-105">Integrating Direct with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="583e9-106">Megadhatja a hozzáférés tooDirect rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="583e9-106">You can control in Azure AD who has access tooDirect</span></span>
- <span data-ttu-id="583e9-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooDirect (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="583e9-107">You can enable your users tooautomatically get signed-on tooDirect (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="583e9-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="583e9-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="583e9-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="583e9-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="583e9-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="583e9-110">Prerequisites</span></span>

<span data-ttu-id="583e9-111">az Azure AD-integrációs tooconfigure közvetlen, a következő elemek hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="583e9-111">tooconfigure Azure AD integration with Direct, you need hello following items:</span></span>

- <span data-ttu-id="583e9-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="583e9-112">An Azure AD subscription</span></span>
- <span data-ttu-id="583e9-113">A közvetlen egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="583e9-113">A Direct single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="583e9-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="583e9-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="583e9-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="583e9-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="583e9-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="583e9-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="583e9-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="583e9-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="583e9-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="583e9-118">Scenario description</span></span>
<span data-ttu-id="583e9-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="583e9-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="583e9-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="583e9-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="583e9-121">Közvetlen hozzáadása hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="583e9-121">Adding Direct from hello gallery</span></span>
2. <span data-ttu-id="583e9-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="583e9-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-direct-from-hello-gallery"></a><span data-ttu-id="583e9-123">Közvetlen hozzáadása hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="583e9-123">Adding Direct from hello gallery</span></span>
<span data-ttu-id="583e9-124">tooconfigure hello integráció közvetlenül az Azure AD-be, meg kell tooadd közvetlen hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="583e9-124">tooconfigure hello integration of Direct into Azure AD, you need tooadd Direct from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="583e9-125">**tooadd közvetlen hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="583e9-125">**tooadd Direct from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="583e9-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="583e9-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="583e9-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="583e9-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="583e9-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="583e9-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="583e9-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="583e9-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="583e9-133">Hello keresési mezőbe, írja be a **közvetlen**.</span><span class="sxs-lookup"><span data-stu-id="583e9-133">In hello search box, type **Direct**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-direct-tutorial/tutorial_direct_search.png)

5. <span data-ttu-id="583e9-135">A hello eredmények panelen válassza ki a **közvetlen**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="583e9-135">In hello results panel, select **Direct**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-direct-tutorial/tutorial_direct_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="583e9-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="583e9-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="583e9-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezést az "Britta Simon." nevű tesztfelhasználó közvetlen alapján</span><span class="sxs-lookup"><span data-stu-id="583e9-138">In this section, you configure and test Azure AD single sign-on with Direct based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="583e9-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó a közvetlen az tooa felhasználó, az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="583e9-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Direct is tooa user in Azure AD.</span></span> <span data-ttu-id="583e9-140">Más szóval hivatkozás közötti kapcsolat egy Azure AD-felhasználó és a kapcsolódó felhasználói hello szereplő létrejött közvetlen igények toobe.</span><span class="sxs-lookup"><span data-stu-id="583e9-140">In other words, a link relationship between an Azure AD user and hello related user in Direct needs toobe established.</span></span>

<span data-ttu-id="583e9-141">Közvetlen, rendelje a hello hello értékét **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="583e9-141">In Direct, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="583e9-142">tooconfigure és tesztelése az Azure AD-egyszeri bejelentkezés közvetlen, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="583e9-142">tooconfigure and test Azure AD single sign-on with Direct, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="583e9-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="583e9-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="583e9-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="583e9-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="583e9-145">**[Közvetlen tesztfelhasználó létrehozása](#creating-a-direct-test-user)**  -toohave egy megfelelője a Britta Simon közvetlen, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="583e9-145">**[Creating a Direct test user](#creating-a-direct-test-user)** - toohave a counterpart of Britta Simon in Direct that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="583e9-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="583e9-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="583e9-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="583e9-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="583e9-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="583e9-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="583e9-149">Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése, és a közvetlen alkalmazás egyszeri bejelentkezés beállítása.</span><span class="sxs-lookup"><span data-stu-id="583e9-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Direct application.</span></span>

<span data-ttu-id="583e9-150">**közvetlen, az Azure AD az egyszeri bejelentkezés tooconfigure hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="583e9-150">**tooconfigure Azure AD single sign-on with Direct, perform hello following steps:**</span></span>

1. <span data-ttu-id="583e9-151">Az Azure portál, a hello hello **közvetlen** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="583e9-151">In hello Azure portal, on hello **Direct** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="583e9-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="583e9-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-direct-tutorial/tutorial_direct_samlbase.png)

3. <span data-ttu-id="583e9-155">A hello **közvetlen tartomány és az URL-címek** szakaszban, ha tooconfigure hello alkalmazás **IDP** kezdeményezett mód:</span><span class="sxs-lookup"><span data-stu-id="583e9-155">On hello **Direct Domain and URLs** section, If you wish tooconfigure hello application in **IDP** initiated mode:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-direct-tutorial/tutorial_direct_url.png)

    <span data-ttu-id="583e9-157">A hello **azonosító** szövegmezőhöz típus hello URL-címe:`https://direct4b.com/`</span><span class="sxs-lookup"><span data-stu-id="583e9-157">In hello **Identifier** textbox, type hello URL: `https://direct4b.com/`</span></span>

4. <span data-ttu-id="583e9-158">Ellenőrizze **megjelenítése speciális URL-beállításainak**, ha tooconfigure hello alkalmazás **SP** kezdeményezett mód:</span><span class="sxs-lookup"><span data-stu-id="583e9-158">Check **Show advanced URL settings**, If you wish tooconfigure hello application in **SP** initiated mode:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-direct-tutorial/tutorial_direct_url1.png)

     <span data-ttu-id="583e9-160">A hello **bejelentkezési URL-cím** szövegmezőhöz típus hello URL-címe:`https://direct4b.com/sso`</span><span class="sxs-lookup"><span data-stu-id="583e9-160">In hello **Sign-on URL** textbox, type hello URL: `https://direct4b.com/sso`</span></span> 
    
5. <span data-ttu-id="583e9-161">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="583e9-161">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-direct-tutorial/tutorial_direct_certificate.png) 

6. <span data-ttu-id="583e9-163">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="583e9-163">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-direct-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="583e9-165">tooconfigure egyszeri bejelentkezést a **közvetlen** oldalon kell letöltött toosend hello **metaadatainak XML-kódja** túl[közvetlen támogatási csoport](https://direct4b.com/ja/support.html#inquiry).</span><span class="sxs-lookup"><span data-stu-id="583e9-165">tooconfigure single sign-on on **Direct** side, you need toosend hello downloaded **Metadata XML** too[Direct support team](https://direct4b.com/ja/support.html#inquiry).</span></span> 

> [!TIP]
> <span data-ttu-id="583e9-166">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="583e9-166">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="583e9-167">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="583e9-167">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="583e9-168">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="583e9-168">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="583e9-169">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="583e9-169">Creating an Azure AD test user</span></span>
<span data-ttu-id="583e9-170">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="583e9-170">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="583e9-172">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="583e9-172">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="583e9-173">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="583e9-173">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-direct-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="583e9-175">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="583e9-175">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-direct-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="583e9-177">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="583e9-177">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-direct-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="583e9-179">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="583e9-179">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-direct-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="583e9-181">a.</span><span class="sxs-lookup"><span data-stu-id="583e9-181">a.</span></span> <span data-ttu-id="583e9-182">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="583e9-182">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="583e9-183">b.</span><span class="sxs-lookup"><span data-stu-id="583e9-183">b.</span></span> <span data-ttu-id="583e9-184">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="583e9-184">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="583e9-185">c.</span><span class="sxs-lookup"><span data-stu-id="583e9-185">c.</span></span> <span data-ttu-id="583e9-186">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="583e9-186">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="583e9-187">d.</span><span class="sxs-lookup"><span data-stu-id="583e9-187">d.</span></span> <span data-ttu-id="583e9-188">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="583e9-188">Click **Create**.</span></span>
 
### <a name="creating-a-direct-test-user"></a><span data-ttu-id="583e9-189">Közvetlen tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="583e9-189">Creating a Direct test user</span></span>

<span data-ttu-id="583e9-190">Ebben a szakaszban egy közvetlen Britta Simon nevű felhasználót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="583e9-190">In this section, you create a user called Britta Simon in Direct.</span></span> <span data-ttu-id="583e9-191">Együttműködve [közvetlen támogatási csoport](https://direct4b.com/ja/support.html#inquiry) felhasználót is hozzáadhat hello hello közvetlen platform.</span><span class="sxs-lookup"><span data-stu-id="583e9-191">Work with [Direct support team](https://direct4b.com/ja/support.html#inquiry) to add hello users in hello Direct platform.</span></span> <span data-ttu-id="583e9-192">Felhasználók kell létrehoznia és aktiválni az egyszeri bejelentkezés használata előtt.</span><span class="sxs-lookup"><span data-stu-id="583e9-192">Users must be created and activated before you use single sign-on.</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="583e9-193">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="583e9-193">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="583e9-194">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooDirect megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="583e9-194">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooDirect.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="583e9-196">**tooassign Britta Simon tooDirect, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="583e9-196">**tooassign Britta Simon tooDirect, perform hello following steps:**</span></span>

1. <span data-ttu-id="583e9-197">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="583e9-197">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="583e9-199">Hello alkalmazások listában válassza ki a **közvetlen**.</span><span class="sxs-lookup"><span data-stu-id="583e9-199">In hello applications list, select **Direct**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-direct-tutorial/tutorial_direct_app.png) 

3. <span data-ttu-id="583e9-201">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="583e9-201">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="583e9-203">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="583e9-203">Click **Add** button.</span></span> <span data-ttu-id="583e9-204">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="583e9-204">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="583e9-206">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="583e9-206">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="583e9-207">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="583e9-207">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="583e9-208">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="583e9-208">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="583e9-209">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="583e9-209">Testing single sign-on</span></span>

<span data-ttu-id="583e9-210">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="583e9-210">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

1. <span data-ttu-id="583e9-211">Ha a tootest **IDP kezdeményezett mód**:</span><span class="sxs-lookup"><span data-stu-id="583e9-211">If you wish tootest in **IDP Initiated Mode**:</span></span>

    <span data-ttu-id="583e9-212">Amikor rákattint hello **közvetlen** csempe az hello hozzáférési panelre, akkor kapja meg automatikusan bejelentkezett tooyour **közvetlen** alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="583e9-212">When you click hello **Direct** tile in hello Access Panel, you should get automatically signed-on tooyour **Direct** application.</span></span>

2. <span data-ttu-id="583e9-213">Ha a tootest **SP kezdeményezett mód**:</span><span class="sxs-lookup"><span data-stu-id="583e9-213">If you wish tootest in **SP Initiated Mode**:</span></span>
    
    <span data-ttu-id="583e9-214">a.</span><span class="sxs-lookup"><span data-stu-id="583e9-214">a.</span></span> <span data-ttu-id="583e9-215">Kattintson a hello **közvetlen** hello hozzáférési Panel csempére, és átirányított toohello alkalmazás bejelentkezési lap.</span><span class="sxs-lookup"><span data-stu-id="583e9-215">Click on hello **Direct** tile in hello Access Panel and you will be redirected toohello application sign-on page.</span></span>

    <span data-ttu-id="583e9-216">b.</span><span class="sxs-lookup"><span data-stu-id="583e9-216">b.</span></span> <span data-ttu-id="583e9-217">Adjon meg a `subdomain` hello szövegmező jelenik meg, és nyomja le az "次へ (Tovább)", és akkor kapja meg automatikusan bejelentkezett tooyour **közvetlen** alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="583e9-217">Input your `subdomain` in hello textbox displayed and press '次へ (Next)' and you should get automatically signed-on tooyour **Direct** application .</span></span>
    
<span data-ttu-id="583e9-218">A hozzáférési Panel kapcsolatos további információkért lásd: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="583e9-218">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="583e9-219">További források</span><span class="sxs-lookup"><span data-stu-id="583e9-219">Additional resources</span></span>

* [<span data-ttu-id="583e9-220">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="583e9-220">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="583e9-221">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="583e9-221">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-direct-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-direct-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-direct-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-direct-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-direct-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-direct-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-direct-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-direct-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-direct-tutorial/tutorial_general_203.png

