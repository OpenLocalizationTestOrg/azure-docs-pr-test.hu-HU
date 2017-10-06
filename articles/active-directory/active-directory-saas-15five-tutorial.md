---
title: "Oktatóanyag: Azure Active Directoryval integrált 15Five |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és 15Five között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 2fb301c2-7d7a-4046-8ee1-7dc9e7684806
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2017
ms.author: jeedes
ms.openlocfilehash: 9e531615c16331ce000e285d13d9adce13735a04
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-15five"></a><span data-ttu-id="f630d-103">Oktatóanyag: Azure Active Directoryval integrált 15Five</span><span class="sxs-lookup"><span data-stu-id="f630d-103">Tutorial: Azure Active Directory integration with 15Five</span></span>

<span data-ttu-id="f630d-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate 15Five az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="f630d-104">In this tutorial, you learn how toointegrate 15Five with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="f630d-105">15Five integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="f630d-105">Integrating 15Five with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="f630d-106">Megadhatja a hozzáférés too15Five rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="f630d-106">You can control in Azure AD who has access too15Five</span></span>
- <span data-ttu-id="f630d-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett too15Five (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="f630d-107">You can enable your users tooautomatically get signed-on too15Five (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="f630d-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="f630d-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="f630d-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="f630d-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f630d-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="f630d-110">Prerequisites</span></span>

<span data-ttu-id="f630d-111">az Azure AD integrálása 15Five tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="f630d-111">tooconfigure Azure AD integration with 15Five, you need hello following items:</span></span>

- <span data-ttu-id="f630d-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="f630d-112">An Azure AD subscription</span></span>
- <span data-ttu-id="f630d-113">Egy 15Five egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="f630d-113">A 15Five single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="f630d-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="f630d-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="f630d-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="f630d-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="f630d-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="f630d-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="f630d-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="f630d-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="f630d-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="f630d-118">Scenario description</span></span>
<span data-ttu-id="f630d-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="f630d-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="f630d-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="f630d-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="f630d-121">Hello gyűjteményből 15Five hozzáadása</span><span class="sxs-lookup"><span data-stu-id="f630d-121">Adding 15Five from hello gallery</span></span>
2. <span data-ttu-id="f630d-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="f630d-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-15five-from-hello-gallery"></a><span data-ttu-id="f630d-123">Hello gyűjteményből 15Five hozzáadása</span><span class="sxs-lookup"><span data-stu-id="f630d-123">Adding 15Five from hello gallery</span></span>
<span data-ttu-id="f630d-124">tooconfigure hello integrációja 15Five az Azure AD-be, meg kell tooadd 15Five hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="f630d-124">tooconfigure hello integration of 15Five into Azure AD, you need tooadd 15Five from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="f630d-125">**tooadd 15Five hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="f630d-125">**tooadd 15Five from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="f630d-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="f630d-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="f630d-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="f630d-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="f630d-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="f630d-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="f630d-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="f630d-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="f630d-133">Hello keresési mezőbe, írja be a **15Five**.</span><span class="sxs-lookup"><span data-stu-id="f630d-133">In hello search box, type **15Five**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-15five-tutorial/tutorial_15five_search.png)

5. <span data-ttu-id="f630d-135">A hello eredmények panelen válassza ki a **15Five**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="f630d-135">In hello results panel, select **15Five**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-15five-tutorial/tutorial_15five_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="f630d-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="f630d-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="f630d-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján 15Five</span><span class="sxs-lookup"><span data-stu-id="f630d-138">In this section, you configure and test Azure AD single sign-on with 15Five based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="f630d-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó 15Five tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="f630d-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in 15Five is tooa user in Azure AD.</span></span> <span data-ttu-id="f630d-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello 15Five közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="f630d-140">In other words, a link relationship between an Azure AD user and hello related user in 15Five needs toobe established.</span></span>

<span data-ttu-id="f630d-141">15Five, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="f630d-141">In 15Five, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="f630d-142">tooconfigure és az Azure AD az egyszeri bejelentkezés 15Five-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="f630d-142">tooconfigure and test Azure AD single sign-on with 15Five, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="f630d-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="f630d-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="f630d-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="f630d-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="f630d-145">**[15Five tesztfelhasználó létrehozása](#creating-a-15five-test-user)**  -toohave Britta Simon egy partner, a 15Five, amely a felhasználó csatolt toohello az Azure AD-ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="f630d-145">**[Creating a 15Five test user](#creating-a-15five-test-user)** - toohave a counterpart of Britta Simon in 15Five that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="f630d-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="f630d-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="f630d-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="f630d-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="f630d-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f630d-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="f630d-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az 15Five alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="f630d-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your 15Five application.</span></span>

<span data-ttu-id="f630d-150">**az Azure AD tooconfigure egyszeri bejelentkezést a 15Five, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="f630d-150">**tooconfigure Azure AD single sign-on with 15Five, perform hello following steps:**</span></span>

1. <span data-ttu-id="f630d-151">Az Azure portál, a hello hello **15Five** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="f630d-151">In hello Azure portal, on hello **15Five** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="f630d-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="f630d-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-15five-tutorial/tutorial_15five_samlbase.png)

3. <span data-ttu-id="f630d-155">A hello **15Five tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="f630d-155">On hello **15Five Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-15five-tutorial/tutorial_15five_url.png)

    <span data-ttu-id="f630d-157">a.</span><span class="sxs-lookup"><span data-stu-id="f630d-157">a.</span></span> <span data-ttu-id="f630d-158">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<companyname>.15five.com`</span><span class="sxs-lookup"><span data-stu-id="f630d-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<companyname>.15five.com`</span></span>

    <span data-ttu-id="f630d-159">b.</span><span class="sxs-lookup"><span data-stu-id="f630d-159">b.</span></span> <span data-ttu-id="f630d-160">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<companyname>.15five.com/saml2/metadata/`</span><span class="sxs-lookup"><span data-stu-id="f630d-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<companyname>.15five.com/saml2/metadata/`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="f630d-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="f630d-161">These values are not real.</span></span> <span data-ttu-id="f630d-162">Frissítse a bejelentkezési URL-cím és azonosító a hello tényleges értékek.</span><span class="sxs-lookup"><span data-stu-id="f630d-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="f630d-163">Ügyfél [15Five ügyfél-támogatási csoport](https://www.15five.com/contact/) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="f630d-163">Contact [15Five Client support team](https://www.15five.com/contact/) tooget these values.</span></span> 
 
4. <span data-ttu-id="f630d-164">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="f630d-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-15five-tutorial/tutorial_15five_certificate.png) 

5. <span data-ttu-id="f630d-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="f630d-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-15five-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="f630d-168">tooconfigure egyszeri bejelentkezést a **15Five** oldalon kell letöltött toosend hello **metaadatainak XML-kódja** túl[15Five támogatási csoport](https://www.15five.com/contact/).</span><span class="sxs-lookup"><span data-stu-id="f630d-168">tooconfigure single sign-on on **15Five** side, you need toosend hello downloaded **Metadata XML** too[15Five support team](https://www.15five.com/contact/).</span></span>

> [!TIP]
> <span data-ttu-id="f630d-169">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="f630d-169">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="f630d-170">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="f630d-170">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="f630d-171">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="f630d-171">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="f630d-172">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="f630d-172">Creating an Azure AD test user</span></span>
<span data-ttu-id="f630d-173">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="f630d-173">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="f630d-175">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="f630d-175">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="f630d-176">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="f630d-176">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-15five-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="f630d-178">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="f630d-178">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-15five-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="f630d-180">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="f630d-180">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-15five-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="f630d-182">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="f630d-182">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-15five-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="f630d-184">a.</span><span class="sxs-lookup"><span data-stu-id="f630d-184">a.</span></span> <span data-ttu-id="f630d-185">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="f630d-185">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="f630d-186">b.</span><span class="sxs-lookup"><span data-stu-id="f630d-186">b.</span></span> <span data-ttu-id="f630d-187">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="f630d-187">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="f630d-188">c.</span><span class="sxs-lookup"><span data-stu-id="f630d-188">c.</span></span> <span data-ttu-id="f630d-189">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="f630d-189">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="f630d-190">d.</span><span class="sxs-lookup"><span data-stu-id="f630d-190">d.</span></span> <span data-ttu-id="f630d-191">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="f630d-191">Click **Create**.</span></span>
 
### <a name="creating-a-15five-test-user"></a><span data-ttu-id="f630d-192">15Five tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="f630d-192">Creating a 15Five test user</span></span>

<span data-ttu-id="f630d-193">az Azure AD tooenable felhasználók toolog a too15Five, akkor ki kell építenie 15Five be.</span><span class="sxs-lookup"><span data-stu-id="f630d-193">tooenable Azure AD users toolog in too15Five, they must be provisioned into 15Five.</span></span> <span data-ttu-id="f630d-194">15Five, ha van egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="f630d-194">When 15Five, provisioning is a manual task.</span></span>

### <a name="tooconfigure-user-provisioning-perform-hello-following-steps"></a><span data-ttu-id="f630d-195">tooconfigure felhasználók átadásához, hajtsa végre a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="f630d-195">tooconfigure user provisioning, perform hello following steps:</span></span>
1. <span data-ttu-id="f630d-196">Jelentkezzen be tooyour **15Five** vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="f630d-196">Log in tooyour **15Five** company site as administrator.</span></span>

2. <span data-ttu-id="f630d-197">Nyissa meg túl**kezelése vállalati**.</span><span class="sxs-lookup"><span data-stu-id="f630d-197">Go too**Manage Company**.</span></span>
   
    <span data-ttu-id="f630d-198">![Kezelheti a vállalat](./media/active-directory-saas-15five-tutorial/IC784675.png "vállalati kezelése")</span><span class="sxs-lookup"><span data-stu-id="f630d-198">![Manage Company](./media/active-directory-saas-15five-tutorial/IC784675.png "Manage Company")</span></span>

3. <span data-ttu-id="f630d-199">Nyissa meg túl**személyek \> hozzáadása személyek**.</span><span class="sxs-lookup"><span data-stu-id="f630d-199">Go too**People \> Add People**.</span></span>
   
    <span data-ttu-id="f630d-200">![Személyek](./media/active-directory-saas-15five-tutorial/IC784676.png "személyek")</span><span class="sxs-lookup"><span data-stu-id="f630d-200">![People](./media/active-directory-saas-15five-tutorial/IC784676.png "People")</span></span>

4. <span data-ttu-id="f630d-201">Hello hozzáadása új személy szakaszban, a hajtsa végre a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="f630d-201">In hello Add New Person section, perform hello following steps:</span></span>
   
    <span data-ttu-id="f630d-202">![Adja hozzá az új személy](./media/active-directory-saas-15five-tutorial/IC784677.png "új személy hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="f630d-202">![Add New Person](./media/active-directory-saas-15five-tutorial/IC784677.png "Add New Person")</span></span>
   
    <span data-ttu-id="f630d-203">a.</span><span class="sxs-lookup"><span data-stu-id="f630d-203">a.</span></span> <span data-ttu-id="f630d-204">Típus hello **Utónév**, **Vezetéknév**, **cím**, **E-mail cím** egy érvényes Azure Active Directory-fiók kívánt tooprovision be hello kapcsolódó szövegmezőből.</span><span class="sxs-lookup"><span data-stu-id="f630d-204">Type hello **First Name**, **Last Name**, **Title**, **Email address** of a valid Azure Active Directory account you want tooprovision into hello related textboxes.</span></span>

    <span data-ttu-id="f630d-205">b.</span><span class="sxs-lookup"><span data-stu-id="f630d-205">b.</span></span> <span data-ttu-id="f630d-206">Kattintson a **Done** (Kész) gombra.</span><span class="sxs-lookup"><span data-stu-id="f630d-206">Click **Done**.</span></span>
   
    > [!NOTE]
    > <span data-ttu-id="f630d-207">hello Azure AD-fiókot tulajdonosa kap egy e-mailt egy hivatkozás tooconfirm hello fiókot is beleértve, mielőtt aktívvá válik.</span><span class="sxs-lookup"><span data-stu-id="f630d-207">hello Azure AD account holder receives an email including a link tooconfirm hello account before it becomes active.</span></span>
   
### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="f630d-208">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="f630d-208">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="f630d-209">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés too15Five megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="f630d-209">In this section, you enable Britta Simon toouse Azure single sign-on by granting access too15Five.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="f630d-211">**tooassign Britta Simon too15Five, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="f630d-211">**tooassign Britta Simon too15Five, perform hello following steps:**</span></span>

1. <span data-ttu-id="f630d-212">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="f630d-212">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="f630d-214">Hello alkalmazások listában válassza ki a **15Five**.</span><span class="sxs-lookup"><span data-stu-id="f630d-214">In hello applications list, select **15Five**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-15five-tutorial/tutorial_15five_app.png) 

3. <span data-ttu-id="f630d-216">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="f630d-216">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="f630d-218">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="f630d-218">Click **Add** button.</span></span> <span data-ttu-id="f630d-219">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="f630d-219">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="f630d-221">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="f630d-221">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="f630d-222">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="f630d-222">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="f630d-223">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="f630d-223">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="f630d-224">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="f630d-224">Testing single sign-on</span></span>

<span data-ttu-id="f630d-225">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="f630d-225">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="f630d-226">Ha a hozzáférési Panel hello hello 15Five csempe gombra kattint, 15Five alkalmazás bejelentkezési oldalán szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="f630d-226">When you click hello 15Five tile in hello Access Panel, you should get login page of 15Five application.</span></span>
<span data-ttu-id="f630d-227">További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="f630d-227">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="f630d-228">További források</span><span class="sxs-lookup"><span data-stu-id="f630d-228">Additional resources</span></span>

* [<span data-ttu-id="f630d-229">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="f630d-229">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="f630d-230">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="f630d-230">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-15five-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-15five-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-15five-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-15five-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-15five-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-15five-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-15five-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-15five-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-15five-tutorial/tutorial_general_203.png

