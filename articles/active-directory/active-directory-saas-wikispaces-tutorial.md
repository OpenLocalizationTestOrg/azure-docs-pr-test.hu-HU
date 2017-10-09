---
title: "Oktatóanyag: Azure Active Directoryval integrált Wikispaces |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Wikispaces között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 665b95aa-f7f5-4406-9e2a-6fc299a1599c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2017
ms.author: jeedes
ms.openlocfilehash: eb5b72e60b415cb657b70ba530df731ae14b0425
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-wikispaces"></a><span data-ttu-id="b6af0-103">Oktatóanyag: Azure Active Directoryval integrált Wikispaces</span><span class="sxs-lookup"><span data-stu-id="b6af0-103">Tutorial: Azure Active Directory integration with Wikispaces</span></span>

<span data-ttu-id="b6af0-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Wikispaces az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="b6af0-104">In this tutorial, you learn how toointegrate Wikispaces with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="b6af0-105">Wikispaces integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="b6af0-105">Integrating Wikispaces with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="b6af0-106">Megadhatja a hozzáférés tooWikispaces rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="b6af0-106">You can control in Azure AD who has access tooWikispaces</span></span>
- <span data-ttu-id="b6af0-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooWikispaces (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="b6af0-107">You can enable your users tooautomatically get signed-on tooWikispaces (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="b6af0-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="b6af0-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="b6af0-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="b6af0-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b6af0-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="b6af0-110">Prerequisites</span></span>

<span data-ttu-id="b6af0-111">az Azure AD integrálása Wikispaces tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="b6af0-111">tooconfigure Azure AD integration with Wikispaces, you need hello following items:</span></span>

- <span data-ttu-id="b6af0-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="b6af0-112">An Azure AD subscription</span></span>
- <span data-ttu-id="b6af0-113">Egy Wikispaces egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="b6af0-113">A Wikispaces single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="b6af0-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="b6af0-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="b6af0-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="b6af0-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="b6af0-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="b6af0-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="b6af0-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="b6af0-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="b6af0-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="b6af0-118">Scenario description</span></span>
<span data-ttu-id="b6af0-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="b6af0-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="b6af0-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="b6af0-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="b6af0-121">Hello gyűjteményből Wikispaces hozzáadása</span><span class="sxs-lookup"><span data-stu-id="b6af0-121">Adding Wikispaces from hello gallery</span></span>
2. <span data-ttu-id="b6af0-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="b6af0-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-wikispaces-from-hello-gallery"></a><span data-ttu-id="b6af0-123">Hello gyűjteményből Wikispaces hozzáadása</span><span class="sxs-lookup"><span data-stu-id="b6af0-123">Adding Wikispaces from hello gallery</span></span>
<span data-ttu-id="b6af0-124">tooconfigure hello integrációja Wikispaces az Azure AD-be, meg kell tooadd Wikispaces hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="b6af0-124">tooconfigure hello integration of Wikispaces into Azure AD, you need tooadd Wikispaces from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="b6af0-125">**tooadd Wikispaces hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="b6af0-125">**tooadd Wikispaces from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="b6af0-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="b6af0-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="b6af0-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="b6af0-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="b6af0-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="b6af0-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="b6af0-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="b6af0-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="b6af0-133">Hello keresési mezőbe, írja be a **Wikispaces**.</span><span class="sxs-lookup"><span data-stu-id="b6af0-133">In hello search box, type **Wikispaces**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-wikispaces-tutorial/tutorial_wikispaces_search.png)

5. <span data-ttu-id="b6af0-135">A hello eredmények panelen válassza ki a **Wikispaces**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="b6af0-135">In hello results panel, select **Wikispaces**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-wikispaces-tutorial/tutorial_wikispaces_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="b6af0-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="b6af0-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="b6af0-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Wikispaces.</span><span class="sxs-lookup"><span data-stu-id="b6af0-138">In this section, you configure and test Azure AD single sign-on with Wikispaces based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="b6af0-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Wikispaces tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="b6af0-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Wikispaces is tooa user in Azure AD.</span></span> <span data-ttu-id="b6af0-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Wikispaces közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="b6af0-140">In other words, a link relationship between an Azure AD user and hello related user in Wikispaces needs toobe established.</span></span>

<span data-ttu-id="b6af0-141">Wikispaces, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="b6af0-141">In Wikispaces, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="b6af0-142">tooconfigure és az Azure AD az egyszeri bejelentkezés Wikispaces-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="b6af0-142">tooconfigure and test Azure AD single sign-on with Wikispaces, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="b6af0-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="b6af0-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="b6af0-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b6af0-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="b6af0-145">**[Wikispaces tesztfelhasználó létrehozása](#creating-a-wikispaces-test-user)**  -toohave egy megfelelője a Britta Simon a Wikispaces, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="b6af0-145">**[Creating a Wikispaces test user](#creating-a-wikispaces-test-user)** - toohave a counterpart of Britta Simon in Wikispaces that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="b6af0-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="b6af0-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="b6af0-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="b6af0-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="b6af0-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b6af0-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="b6af0-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Wikispaces alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="b6af0-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Wikispaces application.</span></span>

<span data-ttu-id="b6af0-150">**az Azure AD tooconfigure egyszeri bejelentkezést a Wikispaces, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="b6af0-150">**tooconfigure Azure AD single sign-on with Wikispaces, perform hello following steps:**</span></span>

1. <span data-ttu-id="b6af0-151">Az Azure portál, a hello hello **Wikispaces** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="b6af0-151">In hello Azure portal, on hello **Wikispaces** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="b6af0-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="b6af0-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-wikispaces-tutorial/tutorial_wikispaces_samlbase.png)

3. <span data-ttu-id="b6af0-155">A hello **Wikispaces tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="b6af0-155">On hello **Wikispaces Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-wikispaces-tutorial/tutorial_wikispaces_url.png)

    <span data-ttu-id="b6af0-157">a.</span><span class="sxs-lookup"><span data-stu-id="b6af0-157">a.</span></span> <span data-ttu-id="b6af0-158">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<companyname>.wikispaces.net`</span><span class="sxs-lookup"><span data-stu-id="b6af0-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<companyname>.wikispaces.net`</span></span>

    <span data-ttu-id="b6af0-159">b.</span><span class="sxs-lookup"><span data-stu-id="b6af0-159">b.</span></span> <span data-ttu-id="b6af0-160">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://session.wikispaces.net/<instancename>`</span><span class="sxs-lookup"><span data-stu-id="b6af0-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://session.wikispaces.net/<instancename>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="b6af0-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="b6af0-161">These values are not real.</span></span> <span data-ttu-id="b6af0-162">Frissítse a bejelentkezési URL-cím és azonosító a hello tényleges értékek.</span><span class="sxs-lookup"><span data-stu-id="b6af0-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="b6af0-163">Ügyfél [Wikispaces ügyfél-támogatási csoport](https://www.wikispaces.com/site/help) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="b6af0-163">Contact [Wikispaces Client support team](https://www.wikispaces.com/site/help) tooget these values.</span></span> 

4. <span data-ttu-id="b6af0-164">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="b6af0-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-wikispaces-tutorial/tutorial_wikispaces_certificate.png) 

5. <span data-ttu-id="b6af0-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="b6af0-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-wikispaces-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="b6af0-168">tooconfigure egyszeri bejelentkezést a **Wikispaces** oldalon kell letöltött toosend hello **metaadatainak XML-kódja** túl[Wikispaces támogatási csoport](https://www.wikispaces.com/site/help).</span><span class="sxs-lookup"><span data-stu-id="b6af0-168">tooconfigure single sign-on on **Wikispaces** side, you need toosend hello downloaded **Metadata XML** too[Wikispaces support team](https://www.wikispaces.com/site/help).</span></span> <span data-ttu-id="b6af0-169">Értesítést kap, amint hello konfigurálása befejeződött.</span><span class="sxs-lookup"><span data-stu-id="b6af0-169">You will get a notification as soon as hello configuration has been completed.</span></span>

> [!TIP]
> <span data-ttu-id="b6af0-170">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="b6af0-170">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="b6af0-171">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="b6af0-171">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="b6af0-172">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="b6af0-172">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="b6af0-173">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="b6af0-173">Creating an Azure AD test user</span></span>
<span data-ttu-id="b6af0-174">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="b6af0-174">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="b6af0-176">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="b6af0-176">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="b6af0-177">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="b6af0-177">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-wikispaces-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="b6af0-179">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="b6af0-179">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-wikispaces-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="b6af0-181">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="b6af0-181">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-wikispaces-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="b6af0-183">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="b6af0-183">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-wikispaces-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="b6af0-185">a.</span><span class="sxs-lookup"><span data-stu-id="b6af0-185">a.</span></span> <span data-ttu-id="b6af0-186">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="b6af0-186">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="b6af0-187">b.</span><span class="sxs-lookup"><span data-stu-id="b6af0-187">b.</span></span> <span data-ttu-id="b6af0-188">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="b6af0-188">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="b6af0-189">c.</span><span class="sxs-lookup"><span data-stu-id="b6af0-189">c.</span></span> <span data-ttu-id="b6af0-190">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="b6af0-190">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="b6af0-191">d.</span><span class="sxs-lookup"><span data-stu-id="b6af0-191">d.</span></span> <span data-ttu-id="b6af0-192">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="b6af0-192">Click **Create**.</span></span>
 
### <a name="creating-a-wikispaces-test-user"></a><span data-ttu-id="b6af0-193">Wikispaces tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="b6af0-193">Creating a Wikispaces test user</span></span>

<span data-ttu-id="b6af0-194">A sorrend tooenable az Azure AD felhasználók toolog a tooWikispaces azok ki kell építenie Wikispaces be.</span><span class="sxs-lookup"><span data-stu-id="b6af0-194">In order tooenable Azure AD users toolog in tooWikispaces, they must be provisioned into Wikispaces.</span></span> <span data-ttu-id="b6af0-195">Wikispaces hello esetben egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="b6af0-195">In hello case of Wikispaces, provisioning is a manual task.</span></span>

### <a name="tooprovision-a-user-accounts-perform-hello-following-steps"></a><span data-ttu-id="b6af0-196">tooprovision felhasználói fiókok, hajtsa végre hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="b6af0-196">tooprovision a user accounts, perform hello following steps:</span></span>
1. <span data-ttu-id="b6af0-197">Jelentkezzen be tooyour **Wikispaces** vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="b6af0-197">Log in tooyour **Wikispaces** company site as an administrator.</span></span>

2. <span data-ttu-id="b6af0-198">Nyissa meg túl**tagok**.</span><span class="sxs-lookup"><span data-stu-id="b6af0-198">Go too**Members**.</span></span>
   
    <span data-ttu-id="b6af0-199">![Tagok](./media/active-directory-saas-wikispaces-tutorial/ic787193.png "tagok")</span><span class="sxs-lookup"><span data-stu-id="b6af0-199">![Members](./media/active-directory-saas-wikispaces-tutorial/ic787193.png "Members")</span></span>

3. <span data-ttu-id="b6af0-200">Kattintson a hello **meghívása személyek**.</span><span class="sxs-lookup"><span data-stu-id="b6af0-200">Click hello **Invite People**.</span></span>
   
    <span data-ttu-id="b6af0-201">![Felkérése](./media/active-directory-saas-wikispaces-tutorial/ic787194.png "felkérése")</span><span class="sxs-lookup"><span data-stu-id="b6af0-201">![Invite People](./media/active-directory-saas-wikispaces-tutorial/ic787194.png "Invite People")</span></span>

4. <span data-ttu-id="b6af0-202">A hello **meghívása személyek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="b6af0-202">In hello **Invite People** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="b6af0-203">![Felkérése](./media/active-directory-saas-wikispaces-tutorial/ic787208.png "felkérése")</span><span class="sxs-lookup"><span data-stu-id="b6af0-203">![Invite People](./media/active-directory-saas-wikispaces-tutorial/ic787208.png "Invite People")</span></span>
   
    <span data-ttu-id="b6af0-204">a.</span><span class="sxs-lookup"><span data-stu-id="b6af0-204">a.</span></span> <span data-ttu-id="b6af0-205">Típus hello **felhasználónevek vagy E-mail cím** a egy érvényes AAD-fiókba, a kívánt tooprovision hello kapcsolódó szövegmezőből.</span><span class="sxs-lookup"><span data-stu-id="b6af0-205">Type hello **Usernames or Email Address** of a valid AAD account you want tooprovision into hello related textboxes.</span></span>
   
    <span data-ttu-id="b6af0-206">b.</span><span class="sxs-lookup"><span data-stu-id="b6af0-206">b.</span></span> <span data-ttu-id="b6af0-207">Kattintson a **küldése**.</span><span class="sxs-lookup"><span data-stu-id="b6af0-207">Click **Send**.</span></span>  
      
    > [!NOTE]
    > <span data-ttu-id="b6af0-208">hello Azure Active Directory fióktulajdonos kap egy e-mailt egy hivatkozás tooconfirm hello fiókot is beleértve, mielőtt aktívvá válik.</span><span class="sxs-lookup"><span data-stu-id="b6af0-208">hello Azure Active Directory account holder receives an email including a link tooconfirm hello account before it becomes active.</span></span>
    
> [!NOTE]
> <span data-ttu-id="b6af0-209">Bármely más Wikispaces felhasználói fiók létrehozása eszközök vagy Wikispaces tooprovision által nyújtott API-k AAD felhasználói fiókokat.</span><span class="sxs-lookup"><span data-stu-id="b6af0-209">You can use any other Wikispaces user account creation tools or APIs provided by Wikispaces tooprovision AAD user accounts.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="b6af0-210">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="b6af0-210">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="b6af0-211">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooWikispaces megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="b6af0-211">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooWikispaces.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="b6af0-213">**tooassign Britta Simon tooWikispaces, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="b6af0-213">**tooassign Britta Simon tooWikispaces, perform hello following steps:**</span></span>

1. <span data-ttu-id="b6af0-214">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="b6af0-214">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="b6af0-216">Hello alkalmazások listában válassza ki a **Wikispaces**.</span><span class="sxs-lookup"><span data-stu-id="b6af0-216">In hello applications list, select **Wikispaces**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-wikispaces-tutorial/tutorial_wikispaces_app.png) 

3. <span data-ttu-id="b6af0-218">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="b6af0-218">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="b6af0-220">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="b6af0-220">Click **Add** button.</span></span> <span data-ttu-id="b6af0-221">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="b6af0-221">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="b6af0-223">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="b6af0-223">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="b6af0-224">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="b6af0-224">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="b6af0-225">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="b6af0-225">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="b6af0-226">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="b6af0-226">Testing single sign-on</span></span>

<span data-ttu-id="b6af0-227">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="b6af0-227">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="b6af0-228">Hello Wikispaces hello hozzáférési Panel csempére kattintva kapja meg automatikusan bejelentkezett tooyour Wikispaces alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="b6af0-228">When you click hello Wikispaces tile in hello Access Panel, you should get automatically signed-on tooyour Wikispaces application.</span></span>
<span data-ttu-id="b6af0-229">További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="b6af0-229">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b6af0-230">További források</span><span class="sxs-lookup"><span data-stu-id="b6af0-230">Additional resources</span></span>

* [<span data-ttu-id="b6af0-231">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="b6af0-231">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="b6af0-232">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="b6af0-232">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-wikispaces-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-wikispaces-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-wikispaces-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-wikispaces-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-wikispaces-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-wikispaces-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-wikispaces-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-wikispaces-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-wikispaces-tutorial/tutorial_general_203.png

