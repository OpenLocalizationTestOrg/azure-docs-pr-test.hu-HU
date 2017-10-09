---
title: "Oktatóanyag: Azure Active Directoryval integrált 360 Online |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és 360 Online között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: cda8eba6-843f-4a09-8c55-0aaf6e593d75
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2017
ms.author: jeedes
ms.openlocfilehash: 413e4e2c41336f99e1999857c788c5dac15be4c4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-360-online"></a><span data-ttu-id="012d1-103">Oktatóanyag: Azure Active Directoryval integrált 360 Online</span><span class="sxs-lookup"><span data-stu-id="012d1-103">Tutorial: Azure Active Directory integration with 360 Online</span></span>

<span data-ttu-id="012d1-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate 360 Online az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="012d1-104">In this tutorial, you learn how toointegrate 360 Online with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="012d1-105">360 Online integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="012d1-105">Integrating 360 Online with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="012d1-106">Megadhatja a hozzáférés too360 Online rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="012d1-106">You can control in Azure AD who has access too360 Online</span></span>
- <span data-ttu-id="012d1-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett too360 Online (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="012d1-107">You can enable your users tooautomatically get signed-on too360 Online (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="012d1-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="012d1-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="012d1-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="012d1-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="012d1-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="012d1-110">Prerequisites</span></span>

<span data-ttu-id="012d1-111">360 Online az Azure AD integrálása tooconfigure, a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="012d1-111">tooconfigure Azure AD integration with 360 Online, you need hello following items:</span></span>

- <span data-ttu-id="012d1-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="012d1-112">An Azure AD subscription</span></span>
- <span data-ttu-id="012d1-113">A 360 Online egyszeri bejelentkezést előfizetés engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="012d1-113">A 360 Online single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="012d1-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="012d1-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="012d1-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="012d1-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="012d1-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="012d1-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="012d1-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="012d1-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="012d1-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="012d1-118">Scenario description</span></span>
<span data-ttu-id="012d1-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="012d1-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="012d1-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="012d1-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="012d1-121">360 Online hozzáadása hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="012d1-121">Adding 360 Online from hello gallery</span></span>
2. <span data-ttu-id="012d1-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="012d1-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-360-online-from-hello-gallery"></a><span data-ttu-id="012d1-123">360 Online hozzáadása hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="012d1-123">Adding 360 Online from hello gallery</span></span>
<span data-ttu-id="012d1-124">tooconfigure hello integrációja 360 Online az Azure AD-be, meg kell tooadd 360 Online hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="012d1-124">tooconfigure hello integration of 360 Online into Azure AD, you need tooadd 360 Online from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="012d1-125">**tooadd 360 Online hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="012d1-125">**tooadd 360 Online from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="012d1-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="012d1-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="012d1-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="012d1-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="012d1-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="012d1-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="012d1-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="012d1-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="012d1-133">Hello keresési mezőbe, írja be a **360 Online**.</span><span class="sxs-lookup"><span data-stu-id="012d1-133">In hello search box, type **360 Online**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-360online-tutorial/tutorial_360online_search.png)

5. <span data-ttu-id="012d1-135">A hello eredmények panelen válassza ki a **360 Online**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="012d1-135">In hello results panel, select **360 Online**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-360online-tutorial/tutorial_360online_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="012d1-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="012d1-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="012d1-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezést az úgynevezett "Britta Simon." tesztfelhasználó alapján 360 Online</span><span class="sxs-lookup"><span data-stu-id="012d1-138">In this section, you configure and test Azure AD single sign-on with 360 Online based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="012d1-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó 360 Online tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="012d1-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in 360 Online is tooa user in Azure AD.</span></span> <span data-ttu-id="012d1-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello 360 hivatkozás kapcsolatának Online kell toobe létrejött.</span><span class="sxs-lookup"><span data-stu-id="012d1-140">In other words, a link relationship between an Azure AD user and hello related user in 360 Online needs toobe established.</span></span>

<span data-ttu-id="012d1-141">360 Online, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="012d1-141">In 360 Online, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="012d1-142">tooconfigure és 360 Online az Azure AD az egyszeri bejelentkezés teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="012d1-142">tooconfigure and test Azure AD single sign-on with 360 Online, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="012d1-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="012d1-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="012d1-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="012d1-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="012d1-145">**[360 Online tesztfelhasználó létrehozása](#creating-a-360-online-test-user)**  -toohave egy megfelelője a Britta Simon a 360 Online, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="012d1-145">**[Creating a 360 Online test user](#creating-a-360-online-test-user)** - toohave a counterpart of Britta Simon in 360 Online that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="012d1-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="012d1-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="012d1-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="012d1-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="012d1-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="012d1-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="012d1-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az 360 Online alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="012d1-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your 360 Online application.</span></span>

<span data-ttu-id="012d1-150">**az Azure AD tooconfigure egyszeri bejelentkezés 360 Online, a hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="012d1-150">**tooconfigure Azure AD single sign-on with 360 Online, perform hello following steps:**</span></span>

1. <span data-ttu-id="012d1-151">Az Azure portál, a hello hello **360 Online** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="012d1-151">In hello Azure portal, on hello **360 Online** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="012d1-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="012d1-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-360online-tutorial/tutorial_360online_samlbase.png)

3. <span data-ttu-id="012d1-155">A hello **360 Online tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="012d1-155">On hello **360 Online Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-360online-tutorial/tutorial_360online_url.png)

    <span data-ttu-id="012d1-157">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<company name>.public360online.com`</span><span class="sxs-lookup"><span data-stu-id="012d1-157">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<company name>.public360online.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="012d1-158">hello érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="012d1-158">hello value is not real.</span></span> <span data-ttu-id="012d1-159">Frissítés hello értékének hello tényleges bejelentkezési URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="012d1-159">Update hello value with hello actual Sign-On URL.</span></span> <span data-ttu-id="012d1-160">Ügyfél [360 Online ügyfél-támogatási csoport](mailto:360online@software-innovation.com) tooget hello érték.</span><span class="sxs-lookup"><span data-stu-id="012d1-160">Contact [360 Online Client support team](mailto:360online@software-innovation.com) tooget hello value.</span></span> 
 
4. <span data-ttu-id="012d1-161">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="012d1-161">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-360online-tutorial/tutorial_360online_certificate.png) 

5. <span data-ttu-id="012d1-163">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="012d1-163">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-360online-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="012d1-165">tooconfigure egyszeri bejelentkezést a **360 Online** oldalon kell letöltött toosend hello **metaadatainak XML-kódja** túl[360 Online támogatási csoport](mailto:360online@software-innovation.com).</span><span class="sxs-lookup"><span data-stu-id="012d1-165">tooconfigure single sign-on on **360 Online** side, you need toosend hello downloaded **Metadata XML** too[360 Online support team](mailto:360online@software-innovation.com).</span></span> 

> [!TIP]
> <span data-ttu-id="012d1-166">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="012d1-166">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="012d1-167">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="012d1-167">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="012d1-168">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="012d1-168">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="012d1-169">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="012d1-169">Creating an Azure AD test user</span></span>
<span data-ttu-id="012d1-170">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="012d1-170">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="012d1-172">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="012d1-172">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="012d1-173">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="012d1-173">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-360online-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="012d1-175">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="012d1-175">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-360online-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="012d1-177">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="012d1-177">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-360online-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="012d1-179">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="012d1-179">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-360online-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="012d1-181">a.</span><span class="sxs-lookup"><span data-stu-id="012d1-181">a.</span></span> <span data-ttu-id="012d1-182">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="012d1-182">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="012d1-183">b.</span><span class="sxs-lookup"><span data-stu-id="012d1-183">b.</span></span> <span data-ttu-id="012d1-184">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="012d1-184">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="012d1-185">c.</span><span class="sxs-lookup"><span data-stu-id="012d1-185">c.</span></span> <span data-ttu-id="012d1-186">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="012d1-186">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="012d1-187">d.</span><span class="sxs-lookup"><span data-stu-id="012d1-187">d.</span></span> <span data-ttu-id="012d1-188">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="012d1-188">Click **Create**.</span></span>
 
### <a name="creating-a-360-online-test-user"></a><span data-ttu-id="012d1-189">360 Online tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="012d1-189">Creating a 360 Online test user</span></span>

<span data-ttu-id="012d1-190">Ebben a szakaszban egy 360 Online Britta Simon nevű felhasználót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="012d1-190">In this section, you create a user called Britta Simon in 360 Online.</span></span> <span data-ttu-id="012d1-191">toocontact kell [360 Online támogatási csoport](mailto:360online@software-innovation.com).</span><span class="sxs-lookup"><span data-stu-id="012d1-191">you need toocontact [360 Online support team](mailto:360online@software-innovation.com).</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="012d1-192">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="012d1-192">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="012d1-193">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés Online hozzáférés too360 megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="012d1-193">In this section, you enable Britta Simon toouse Azure single sign-on by granting access too360 Online.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="012d1-195">**tooassign Britta Simon too360 Online, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="012d1-195">**tooassign Britta Simon too360 Online, perform hello following steps:**</span></span>

1. <span data-ttu-id="012d1-196">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="012d1-196">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="012d1-198">Hello alkalmazások listában válassza ki a **360 Online**.</span><span class="sxs-lookup"><span data-stu-id="012d1-198">In hello applications list, select **360 Online**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-360online-tutorial/tutorial_360online_app.png) 

3. <span data-ttu-id="012d1-200">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="012d1-200">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="012d1-202">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="012d1-202">Click **Add** button.</span></span> <span data-ttu-id="012d1-203">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="012d1-203">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="012d1-205">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="012d1-205">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="012d1-206">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="012d1-206">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="012d1-207">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="012d1-207">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="012d1-208">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="012d1-208">Testing single sign-on</span></span>

<span data-ttu-id="012d1-209">hello ebben a szakaszban célja tootest az egyszeri bejelentkezés konfigurációs használatával hello a hozzáférési Panel.</span><span class="sxs-lookup"><span data-stu-id="012d1-209">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="012d1-210">Hello 360 Online kattintáskor csempe hello hozzáférési Panel az Online alkalmazás automatikusan bejelentkezett tooyour 360 szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="012d1-210">When you click hello 360 Online tile in hello Access Panel, you should get automatically signed-on tooyour 360 Online application.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="012d1-211">További források</span><span class="sxs-lookup"><span data-stu-id="012d1-211">Additional resources</span></span>

* [<span data-ttu-id="012d1-212">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="012d1-212">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="012d1-213">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="012d1-213">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-360online-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-360online-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-360online-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-360online-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-360online-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-360online-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-360online-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-360online-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-360online-tutorial/tutorial_general_203.png

