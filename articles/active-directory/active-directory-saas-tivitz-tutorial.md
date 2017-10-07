---
title: "Oktatóanyag: Azure Active Directoryval integrált TiViTz |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és TiViTz között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: b97ed88f-7888-4185-be22-41d1f62cbbf1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: jeedes
ms.openlocfilehash: e915c31d317c4713720a4db07b23764d91f26a6e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-tivitz"></a><span data-ttu-id="975d4-103">Oktatóanyag: Azure Active Directoryval integrált TiViTz</span><span class="sxs-lookup"><span data-stu-id="975d4-103">Tutorial: Azure Active Directory integration with TiViTz</span></span>

<span data-ttu-id="975d4-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate TiViTz az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="975d4-104">In this tutorial, you learn how toointegrate TiViTz with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="975d4-105">TiViTz integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="975d4-105">Integrating TiViTz with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="975d4-106">Megadhatja a hozzáférés tooTiViTz rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="975d4-106">You can control in Azure AD who has access tooTiViTz</span></span>
- <span data-ttu-id="975d4-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooTiViTz (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="975d4-107">You can enable your users tooautomatically get signed-on tooTiViTz (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="975d4-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="975d4-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="975d4-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="975d4-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="975d4-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="975d4-110">Prerequisites</span></span>

<span data-ttu-id="975d4-111">az Azure AD integrálása TiViTz tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="975d4-111">tooconfigure Azure AD integration with TiViTz, you need hello following items:</span></span>

- <span data-ttu-id="975d4-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="975d4-112">An Azure AD subscription</span></span>
- <span data-ttu-id="975d4-113">Egy TiViTz egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="975d4-113">A TiViTz single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="975d4-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="975d4-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="975d4-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="975d4-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="975d4-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="975d4-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="975d4-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="975d4-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="975d4-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="975d4-118">Scenario description</span></span>
<span data-ttu-id="975d4-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="975d4-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="975d4-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="975d4-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="975d4-121">Hello gyűjteményből TiViTz hozzáadása</span><span class="sxs-lookup"><span data-stu-id="975d4-121">Adding TiViTz from hello gallery</span></span>
2. <span data-ttu-id="975d4-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="975d4-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-tivitz-from-hello-gallery"></a><span data-ttu-id="975d4-123">Hello gyűjteményből TiViTz hozzáadása</span><span class="sxs-lookup"><span data-stu-id="975d4-123">Adding TiViTz from hello gallery</span></span>
<span data-ttu-id="975d4-124">tooconfigure hello integrációja TiViTz az Azure AD-be, meg kell tooadd TiViTz hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="975d4-124">tooconfigure hello integration of TiViTz into Azure AD, you need tooadd TiViTz from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="975d4-125">**tooadd TiViTz hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="975d4-125">**tooadd TiViTz from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="975d4-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="975d4-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="975d4-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="975d4-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="975d4-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="975d4-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="975d4-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="975d4-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="975d4-133">Hello keresési mezőbe, írja be a **TiViTz**.</span><span class="sxs-lookup"><span data-stu-id="975d4-133">In hello search box, type **TiViTz**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-tivitz-tutorial/tutorial_tivitz_search.png)

5. <span data-ttu-id="975d4-135">A hello eredmények panelen válassza ki a **TiViTz**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="975d4-135">In hello results panel, select **TiViTz**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-tivitz-tutorial/tutorial_tivitz_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="975d4-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="975d4-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="975d4-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján TiViTz.</span><span class="sxs-lookup"><span data-stu-id="975d4-138">In this section, you configure and test Azure AD single sign-on with TiViTz based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="975d4-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó TiViTz tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="975d4-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in TiViTz is tooa user in Azure AD.</span></span> <span data-ttu-id="975d4-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello TiViTz közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="975d4-140">In other words, a link relationship between an Azure AD user and hello related user in TiViTz needs toobe established.</span></span>

<span data-ttu-id="975d4-141">TiViTz, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="975d4-141">In TiViTz, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="975d4-142">tooconfigure és az Azure AD az egyszeri bejelentkezés TiViTz-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="975d4-142">tooconfigure and test Azure AD single sign-on with TiViTz, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="975d4-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="975d4-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="975d4-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="975d4-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="975d4-145">**[TiViTz tesztfelhasználó létrehozása](#creating-a-tivitz-test-user)**  -toohave egy megfelelője a Britta Simon a TiViTz, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="975d4-145">**[Creating a TiViTz test user](#creating-a-tivitz-test-user)** - toohave a counterpart of Britta Simon in TiViTz that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="975d4-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="975d4-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="975d4-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="975d4-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="975d4-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="975d4-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="975d4-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az TiViTz alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="975d4-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your TiViTz application.</span></span>

<span data-ttu-id="975d4-150">**az Azure AD tooconfigure egyszeri bejelentkezést a TiViTz, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="975d4-150">**tooconfigure Azure AD single sign-on with TiViTz, perform hello following steps:**</span></span>

1. <span data-ttu-id="975d4-151">Az Azure portál, a hello hello **TiViTz** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="975d4-151">In hello Azure portal, on hello **TiViTz** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="975d4-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="975d4-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-tivitz-tutorial/tutorial_tivitz_samlbase.png)

3. <span data-ttu-id="975d4-155">A hello **TiViTz tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="975d4-155">On hello **TiViTz Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-tivitz-tutorial/tutorial_tivitz_url.png)

    <span data-ttu-id="975d4-157">a.</span><span class="sxs-lookup"><span data-stu-id="975d4-157">a.</span></span> <span data-ttu-id="975d4-158">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<companyname>.o365.tivitz.com/`</span><span class="sxs-lookup"><span data-stu-id="975d4-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<companyname>.o365.tivitz.com/`</span></span>

    <span data-ttu-id="975d4-159">b.</span><span class="sxs-lookup"><span data-stu-id="975d4-159">b.</span></span> <span data-ttu-id="975d4-160">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<companyname>.o365.tivitz.com/`</span><span class="sxs-lookup"><span data-stu-id="975d4-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<companyname>.o365.tivitz.com/`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="975d4-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="975d4-161">These values are not real.</span></span> <span data-ttu-id="975d4-162">Frissítse a bejelentkezési URL-cím és azonosító a hello tényleges értékek.</span><span class="sxs-lookup"><span data-stu-id="975d4-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="975d4-163">Ügyfél [TiViTz ügyfél-támogatási csoport](mailto:info@tivitz.com) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="975d4-163">Contact [TiViTz Client support team](mailto:info@tivitz.com) tooget these values.</span></span> 

4. <span data-ttu-id="975d4-164">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="975d4-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-tivitz-tutorial/tutorial_tivitz_certificate.png) 

5. <span data-ttu-id="975d4-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="975d4-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-tivitz-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="975d4-168">tooconfigure egyszeri bejelentkezést a **TiViTz** oldalon kell letöltött toosend hello **metaadatainak XML-kódja** túl[TiViTz támogatási csoport](mailto:info@tivitz.com).</span><span class="sxs-lookup"><span data-stu-id="975d4-168">tooconfigure single sign-on on **TiViTz** side, you need toosend hello downloaded **Metadata XML** too[TiViTz support team](mailto:info@tivitz.com).</span></span>

> [!TIP]
> <span data-ttu-id="975d4-169">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="975d4-169">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="975d4-170">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="975d4-170">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="975d4-171">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="975d4-171">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="975d4-172">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="975d4-172">Creating an Azure AD test user</span></span>
<span data-ttu-id="975d4-173">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="975d4-173">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="975d4-175">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="975d4-175">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="975d4-176">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="975d4-176">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-tivitz-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="975d4-178">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="975d4-178">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-tivitz-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="975d4-180">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="975d4-180">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-tivitz-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="975d4-182">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="975d4-182">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-tivitz-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="975d4-184">a.</span><span class="sxs-lookup"><span data-stu-id="975d4-184">a.</span></span> <span data-ttu-id="975d4-185">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="975d4-185">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="975d4-186">b.</span><span class="sxs-lookup"><span data-stu-id="975d4-186">b.</span></span> <span data-ttu-id="975d4-187">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="975d4-187">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="975d4-188">c.</span><span class="sxs-lookup"><span data-stu-id="975d4-188">c.</span></span> <span data-ttu-id="975d4-189">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="975d4-189">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="975d4-190">d.</span><span class="sxs-lookup"><span data-stu-id="975d4-190">d.</span></span> <span data-ttu-id="975d4-191">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="975d4-191">Click **Create**.</span></span>
 
### <a name="creating-a-tivitz-test-user"></a><span data-ttu-id="975d4-192">TiViTz tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="975d4-192">Creating a TiViTz test user</span></span>

<span data-ttu-id="975d4-193">hello ebben a szakaszban célja toocreate TiViTz Britta Simon nevű felhasználó.</span><span class="sxs-lookup"><span data-stu-id="975d4-193">hello objective of this section is toocreate a user called Britta Simon in TiViTz.</span></span> <span data-ttu-id="975d4-194">TiViTz támogatja just-in-time kiosztást, amely alapértelmezés szerint van engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="975d4-194">TiViTz supports just-in-time provisioning, which is by default enabled.</span></span> <span data-ttu-id="975d4-195">Nincs ebben a szakaszban az Ön művelet elem.</span><span class="sxs-lookup"><span data-stu-id="975d4-195">There is no action item for you in this section.</span></span> <span data-ttu-id="975d4-196">Új felhasználó jön létre egy kísérlet tooaccess TiViTz során, ha még nem létezik.</span><span class="sxs-lookup"><span data-stu-id="975d4-196">A new user is created during an attempt tooaccess TiViTz if it doesn't exist yet.</span></span>

>[!NOTE]
><span data-ttu-id="975d4-197">Ha egy felhasználó toocreate manuálisan kell, toocontact kell [TiViTz támogatási csoport](mailto:info@tivitz.com).</span><span class="sxs-lookup"><span data-stu-id="975d4-197">If you need toocreate an user manually, you need toocontact [TiViTz support team](mailto:info@tivitz.com).</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="975d4-198">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="975d4-198">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="975d4-199">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooTiViTz megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="975d4-199">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooTiViTz.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="975d4-201">**tooassign Britta Simon tooTiViTz, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="975d4-201">**tooassign Britta Simon tooTiViTz, perform hello following steps:**</span></span>

1. <span data-ttu-id="975d4-202">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="975d4-202">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="975d4-204">Hello alkalmazások listában válassza ki a **TiViTz**.</span><span class="sxs-lookup"><span data-stu-id="975d4-204">In hello applications list, select **TiViTz**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-tivitz-tutorial/tutorial_tivitz_app.png) 

3. <span data-ttu-id="975d4-206">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="975d4-206">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="975d4-208">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="975d4-208">Click **Add** button.</span></span> <span data-ttu-id="975d4-209">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="975d4-209">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="975d4-211">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="975d4-211">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="975d4-212">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="975d4-212">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="975d4-213">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="975d4-213">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="975d4-214">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="975d4-214">Testing single sign-on</span></span>

<span data-ttu-id="975d4-215">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="975d4-215">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="975d4-216">Ha a hozzáférési Panel hello hello TiViTz csempe gombra kattint, automatikusan bejelentkezett tooyour TiViTz alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="975d4-216">When you click hello TiViTz tile in hello Access Panel, you should get automatically signed-on tooyour TiViTz application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="975d4-217">További források</span><span class="sxs-lookup"><span data-stu-id="975d4-217">Additional resources</span></span>

* [<span data-ttu-id="975d4-218">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="975d4-218">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="975d4-219">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="975d4-219">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-tivitz-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-tivitz-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-tivitz-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-tivitz-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-tivitz-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-tivitz-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-tivitz-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-tivitz-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-tivitz-tutorial/tutorial_general_203.png

