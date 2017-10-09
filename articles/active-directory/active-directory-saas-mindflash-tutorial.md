---
title: "Oktatóanyag: Azure Active Directoryval integrált Mindflash |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Mindflash között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: bdf91993-aaaa-4598-89b7-77ef8ca065d5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes
ms.openlocfilehash: a1bc327ea3867287103acbb64d30f0a8d7d4c5e9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-mindflash"></a><span data-ttu-id="3d128-103">Oktatóanyag: Azure Active Directoryval integrált Mindflash</span><span class="sxs-lookup"><span data-stu-id="3d128-103">Tutorial: Azure Active Directory integration with Mindflash</span></span>

<span data-ttu-id="3d128-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Mindflash az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="3d128-104">In this tutorial, you learn how toointegrate Mindflash with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="3d128-105">Mindflash integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="3d128-105">Integrating Mindflash with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="3d128-106">Megadhatja a hozzáférés tooMindflash rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="3d128-106">You can control in Azure AD who has access tooMindflash</span></span>
- <span data-ttu-id="3d128-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooMindflash (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="3d128-107">You can enable your users tooautomatically get signed-on tooMindflash (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="3d128-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="3d128-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="3d128-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="3d128-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3d128-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="3d128-110">Prerequisites</span></span>

<span data-ttu-id="3d128-111">az Azure AD integrálása Mindflash tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="3d128-111">tooconfigure Azure AD integration with Mindflash, you need hello following items:</span></span>

- <span data-ttu-id="3d128-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="3d128-112">An Azure AD subscription</span></span>
- <span data-ttu-id="3d128-113">Egy Mindflash egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="3d128-113">A Mindflash single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="3d128-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="3d128-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="3d128-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="3d128-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="3d128-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="3d128-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="3d128-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="3d128-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="3d128-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="3d128-118">Scenario description</span></span>
<span data-ttu-id="3d128-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="3d128-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="3d128-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="3d128-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="3d128-121">Hello gyűjteményből Mindflash hozzáadása</span><span class="sxs-lookup"><span data-stu-id="3d128-121">Adding Mindflash from hello gallery</span></span>
2. <span data-ttu-id="3d128-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="3d128-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-mindflash-from-hello-gallery"></a><span data-ttu-id="3d128-123">Hello gyűjteményből Mindflash hozzáadása</span><span class="sxs-lookup"><span data-stu-id="3d128-123">Adding Mindflash from hello gallery</span></span>
<span data-ttu-id="3d128-124">tooconfigure hello integrációja Mindflash az Azure AD-be, meg kell tooadd Mindflash hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="3d128-124">tooconfigure hello integration of Mindflash into Azure AD, you need tooadd Mindflash from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="3d128-125">**tooadd Mindflash hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="3d128-125">**tooadd Mindflash from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="3d128-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="3d128-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="3d128-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="3d128-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="3d128-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="3d128-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="3d128-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="3d128-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="3d128-133">Hello keresési mezőbe, írja be a **Mindflash**.</span><span class="sxs-lookup"><span data-stu-id="3d128-133">In hello search box, type **Mindflash**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-mindflash-tutorial/tutorial_mindflash_search.png)

5. <span data-ttu-id="3d128-135">A hello eredmények panelen válassza ki a **Mindflash**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="3d128-135">In hello results panel, select **Mindflash**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-mindflash-tutorial/tutorial_mindflash_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="3d128-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="3d128-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="3d128-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Mindflash.</span><span class="sxs-lookup"><span data-stu-id="3d128-138">In this section, you configure and test Azure AD single sign-on with Mindflash based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="3d128-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Mindflash tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="3d128-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Mindflash is tooa user in Azure AD.</span></span> <span data-ttu-id="3d128-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Mindflash közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="3d128-140">In other words, a link relationship between an Azure AD user and hello related user in Mindflash needs toobe established.</span></span>

<span data-ttu-id="3d128-141">Mindflash, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="3d128-141">In Mindflash, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="3d128-142">tooconfigure és az Azure AD az egyszeri bejelentkezés Mindflash-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="3d128-142">tooconfigure and test Azure AD single sign-on with Mindflash, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="3d128-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="3d128-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="3d128-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="3d128-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="3d128-145">**[Mindflash tesztfelhasználó létrehozása](#creating-a-mindflash-test-user)**  -toohave egy megfelelője a Britta Simon a Mindflash, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="3d128-145">**[Creating a Mindflash test user](#creating-a-mindflash-test-user)** - toohave a counterpart of Britta Simon in Mindflash that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="3d128-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="3d128-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="3d128-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="3d128-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="3d128-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="3d128-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="3d128-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Mindflash alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="3d128-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Mindflash application.</span></span>

<span data-ttu-id="3d128-150">**az Azure AD tooconfigure egyszeri bejelentkezést a Mindflash, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="3d128-150">**tooconfigure Azure AD single sign-on with Mindflash, perform hello following steps:**</span></span>

1. <span data-ttu-id="3d128-151">Az Azure portál, a hello hello **Mindflash** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="3d128-151">In hello Azure portal, on hello **Mindflash** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="3d128-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="3d128-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-mindflash-tutorial/tutorial_mindflash_samlbase.png)

3. <span data-ttu-id="3d128-155">A hello **Mindflash tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="3d128-155">On hello **Mindflash Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-mindflash-tutorial/tutorial_mindflash_url.png)

    <span data-ttu-id="3d128-157">a.</span><span class="sxs-lookup"><span data-stu-id="3d128-157">a.</span></span> <span data-ttu-id="3d128-158">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<companyname>.mindflash.com`</span><span class="sxs-lookup"><span data-stu-id="3d128-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<companyname>.mindflash.com`</span></span>

    <span data-ttu-id="3d128-159">b.</span><span class="sxs-lookup"><span data-stu-id="3d128-159">b.</span></span> <span data-ttu-id="3d128-160">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<companyname>.mindflash.com`</span><span class="sxs-lookup"><span data-stu-id="3d128-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<companyname>.mindflash.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="3d128-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="3d128-161">These values are not real.</span></span> <span data-ttu-id="3d128-162">Frissítse a bejelentkezési URL-cím és azonosító a hello tényleges értékek.</span><span class="sxs-lookup"><span data-stu-id="3d128-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="3d128-163">Ügyfél [Mindflash ügyfél-támogatási csoport](https://www.mindflash.com/contact/) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="3d128-163">Contact [Mindflash Client support team](https://www.mindflash.com/contact/) tooget these values.</span></span> 
 


4. <span data-ttu-id="3d128-164">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="3d128-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-mindflash-tutorial/tutorial_mindflash_certificate.png) 

5. <span data-ttu-id="3d128-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="3d128-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-mindflash-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="3d128-168">tooconfigure egyszeri bejelentkezést a **Mindflash** oldalon kell letöltött toosend hello **metaadatainak XML-kódja** túl[Mindflash támogatási csoport](https://www.mindflash.com/contact/).</span><span class="sxs-lookup"><span data-stu-id="3d128-168">tooconfigure single sign-on on **Mindflash** side, you need toosend hello downloaded **Metadata XML** too[Mindflash support team](https://www.mindflash.com/contact/).</span></span>

> [!TIP]
> <span data-ttu-id="3d128-169">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="3d128-169">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="3d128-170">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="3d128-170">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="3d128-171">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="3d128-171">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="3d128-172">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="3d128-172">Creating an Azure AD test user</span></span>
<span data-ttu-id="3d128-173">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="3d128-173">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="3d128-175">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="3d128-175">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="3d128-176">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="3d128-176">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-mindflash-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="3d128-178">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="3d128-178">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-mindflash-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="3d128-180">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="3d128-180">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-mindflash-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="3d128-182">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="3d128-182">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-mindflash-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="3d128-184">a.</span><span class="sxs-lookup"><span data-stu-id="3d128-184">a.</span></span> <span data-ttu-id="3d128-185">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="3d128-185">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="3d128-186">b.</span><span class="sxs-lookup"><span data-stu-id="3d128-186">b.</span></span> <span data-ttu-id="3d128-187">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="3d128-187">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="3d128-188">c.</span><span class="sxs-lookup"><span data-stu-id="3d128-188">c.</span></span> <span data-ttu-id="3d128-189">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="3d128-189">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="3d128-190">d.</span><span class="sxs-lookup"><span data-stu-id="3d128-190">d.</span></span> <span data-ttu-id="3d128-191">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="3d128-191">Click **Create**.</span></span>
 
### <a name="creating-a-mindflash-test-user"></a><span data-ttu-id="3d128-192">Mindflash tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="3d128-192">Creating a Mindflash test user</span></span>

<span data-ttu-id="3d128-193">A sorrend tooenable az Azure AD felhasználók toolog Mindflash be azok ki kell építenie Mindflash be.</span><span class="sxs-lookup"><span data-stu-id="3d128-193">In order tooenable Azure AD users toolog into Mindflash, they must be provisioned into Mindflash.</span></span> <span data-ttu-id="3d128-194">Mindflash hello esetben egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="3d128-194">In hello case of Mindflash, provisioning is a manual task.</span></span>

### <a name="tooprovision-a-user-accounts-perform-hello-following-steps"></a><span data-ttu-id="3d128-195">tooprovision felhasználói fiókok, hajtsa végre hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="3d128-195">tooprovision a user accounts, perform hello following steps:</span></span>

1. <span data-ttu-id="3d128-196">Jelentkezzen be tooyour **Mindflash** vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="3d128-196">Log in tooyour **Mindflash** company site as an administrator.</span></span>

2. <span data-ttu-id="3d128-197">Nyissa meg túl**felhasználók kezelése**.</span><span class="sxs-lookup"><span data-stu-id="3d128-197">Go too**Manage Users**.</span></span>
   
    <span data-ttu-id="3d128-198">![Felhasználók kezelése](./media/active-directory-saas-mindflash-tutorial/ic787140.png "felhasználók kezelése")</span><span class="sxs-lookup"><span data-stu-id="3d128-198">![Manage Users](./media/active-directory-saas-mindflash-tutorial/ic787140.png "Manage Users")</span></span>

3. <span data-ttu-id="3d128-199">Kattintson a hello **felhasználó hozzáadása**, és kattintson a **új**.</span><span class="sxs-lookup"><span data-stu-id="3d128-199">Click hello **Add Users**, and then click **New**.</span></span>

4. <span data-ttu-id="3d128-200">A hello **az új felhasználók hozzáadása** csoportjában hajtsa végre a következő lépéseket egy érvényes Azure hello tooprovision kívánt AD-fiókot:</span><span class="sxs-lookup"><span data-stu-id="3d128-200">In hello **Add New Users** section, perform hello following steps of a valid Azure AD account you want tooprovision:</span></span>
   
    <span data-ttu-id="3d128-201">![Új felhasználók hozzáadása az](./media/active-directory-saas-mindflash-tutorial/ic787141.png "új felhasználók hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="3d128-201">![Add New Users](./media/active-directory-saas-mindflash-tutorial/ic787141.png "Add New Users")</span></span>
   
    <span data-ttu-id="3d128-202">a.</span><span class="sxs-lookup"><span data-stu-id="3d128-202">a.</span></span> <span data-ttu-id="3d128-203">A hello **Utónév** szövegmezőhöz típus **Utónév** hello felhasználó mint **Britta**.</span><span class="sxs-lookup"><span data-stu-id="3d128-203">In hello **First name** textbox, type **First name** of hello user as **Britta**.</span></span>

    <span data-ttu-id="3d128-204">b.</span><span class="sxs-lookup"><span data-stu-id="3d128-204">b.</span></span> <span data-ttu-id="3d128-205">A hello **Vezetéknév** szövegmezőhöz típus **Vezetéknév** hello felhasználó mint **Simon**.</span><span class="sxs-lookup"><span data-stu-id="3d128-205">In hello **Last name** textbox, type **Last name** of hello user as **Simon**.</span></span>
    
    <span data-ttu-id="3d128-206">c.</span><span class="sxs-lookup"><span data-stu-id="3d128-206">c.</span></span> <span data-ttu-id="3d128-207">A hello **E-mail** szövegmezőhöz típus **E-mail cím** hello felhasználó mint  **BrittaSimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="3d128-207">In hello **Email** textbox, type **Email Address** of hello user as **BrittaSimon@contoso.com**.</span></span>

    <span data-ttu-id="3d128-208">b.</span><span class="sxs-lookup"><span data-stu-id="3d128-208">b.</span></span> <span data-ttu-id="3d128-209">Kattintson az **Add** (Hozzáadás) parancsra.</span><span class="sxs-lookup"><span data-stu-id="3d128-209">Click **Add**.</span></span>

>[!NOTE]
><span data-ttu-id="3d128-210">Bármely más Mindflash felhasználói fiók létrehozása eszközök vagy Mindflash tooprovision által nyújtott API-k AAD felhasználói fiókokat.</span><span class="sxs-lookup"><span data-stu-id="3d128-210">You can use any other Mindflash user account creation tools or APIs provided by Mindflash tooprovision AAD user accounts.</span></span> 
> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="3d128-211">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="3d128-211">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="3d128-212">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooMindflash megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="3d128-212">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooMindflash.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="3d128-214">**tooassign Britta Simon tooMindflash, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="3d128-214">**tooassign Britta Simon tooMindflash, perform hello following steps:**</span></span>

1. <span data-ttu-id="3d128-215">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="3d128-215">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="3d128-217">Hello alkalmazások listában válassza ki a **Mindflash**.</span><span class="sxs-lookup"><span data-stu-id="3d128-217">In hello applications list, select **Mindflash**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-mindflash-tutorial/tutorial_mindflash_app.png) 

3. <span data-ttu-id="3d128-219">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="3d128-219">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="3d128-221">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="3d128-221">Click **Add** button.</span></span> <span data-ttu-id="3d128-222">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="3d128-222">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="3d128-224">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="3d128-224">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="3d128-225">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="3d128-225">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="3d128-226">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="3d128-226">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="3d128-227">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="3d128-227">Testing single sign-on</span></span>

<span data-ttu-id="3d128-228">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="3d128-228">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="3d128-229">Ha a hozzáférési Panel hello hello Mindflash csempe gombra kattint, Mindflash alkalmazás bejelentkezési oldalán szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="3d128-229">When you click hello Mindflash tile in hello Access Panel, you should get login page of Mindflash application.</span></span>
<span data-ttu-id="3d128-230">További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="3d128-230">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3d128-231">További források</span><span class="sxs-lookup"><span data-stu-id="3d128-231">Additional resources</span></span>

* [<span data-ttu-id="3d128-232">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="3d128-232">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="3d128-233">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="3d128-233">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-mindflash-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-mindflash-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-mindflash-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-mindflash-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-mindflash-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-mindflash-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-mindflash-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-mindflash-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-mindflash-tutorial/tutorial_general_203.png

