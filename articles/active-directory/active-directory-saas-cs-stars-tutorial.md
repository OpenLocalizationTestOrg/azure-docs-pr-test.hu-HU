---
title: "Oktatóanyag: Azure Active Directoryval integrált CS csillag |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory- és CS csillag között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 5704d151-afb8-40a4-b286-8bacd4f279ee
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: jeedes
ms.openlocfilehash: d84533e8a7e9bca2f7bdf4be7f3050bca2f18496
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-cs-stars"></a><span data-ttu-id="53e56-103">Oktatóanyag: Azure Active Directoryval integrált CS csillag</span><span class="sxs-lookup"><span data-stu-id="53e56-103">Tutorial: Azure Active Directory integration with CS Stars</span></span>

<span data-ttu-id="53e56-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate CS csillaggal az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="53e56-104">In this tutorial, you learn how toointegrate CS Stars with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="53e56-105">CS csillag integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="53e56-105">Integrating CS Stars with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="53e56-106">Szabályozhatja, aki hozzáféréssel rendelkezik az Azure AD-ben tooCS csillag</span><span class="sxs-lookup"><span data-stu-id="53e56-106">You can control in Azure AD who has access tooCS Stars</span></span>
- <span data-ttu-id="53e56-107">Engedélyezheti a felhasználók tooautomatically bejelentkezett tooCS csillag (egyszeri bejelentkezés) az Azure AD-fiókok az beszerzése</span><span class="sxs-lookup"><span data-stu-id="53e56-107">You can enable your users tooautomatically get signed-on tooCS Stars (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="53e56-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="53e56-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="53e56-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="53e56-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="53e56-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="53e56-110">Prerequisites</span></span>

<span data-ttu-id="53e56-111">tooconfigure az Azure AD CS csillag integrációja, a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="53e56-111">tooconfigure Azure AD integration with CS Stars, you need hello following items:</span></span>

- <span data-ttu-id="53e56-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="53e56-112">An Azure AD subscription</span></span>
- <span data-ttu-id="53e56-113">A Tanúsítványszolgáltatások csillag egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="53e56-113">A CS Stars single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="53e56-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="53e56-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="53e56-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="53e56-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="53e56-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="53e56-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="53e56-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="53e56-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="53e56-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="53e56-118">Scenario description</span></span>
<span data-ttu-id="53e56-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="53e56-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="53e56-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="53e56-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="53e56-121">CS csillag hozzáadása hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="53e56-121">Adding CS Stars from hello gallery</span></span>
2. <span data-ttu-id="53e56-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="53e56-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-cs-stars-from-hello-gallery"></a><span data-ttu-id="53e56-123">CS csillag hozzáadása hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="53e56-123">Adding CS Stars from hello gallery</span></span>
<span data-ttu-id="53e56-124">tooconfigure hello integrációja CS csillag az Azure AD-be, meg kell tooadd CS csillag hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="53e56-124">tooconfigure hello integration of CS Stars into Azure AD, you need tooadd CS Stars from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="53e56-125">**tooadd CS csillag hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="53e56-125">**tooadd CS Stars from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="53e56-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="53e56-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="53e56-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="53e56-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="53e56-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="53e56-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="53e56-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="53e56-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="53e56-133">Hello keresési mezőbe, írja be a **CS csillag**.</span><span class="sxs-lookup"><span data-stu-id="53e56-133">In hello search box, type **CS Stars**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-cs-stars-tutorial/tutorial_csstars_search.png)

5. <span data-ttu-id="53e56-135">A hello eredmények panelen válassza ki a **CS csillag**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="53e56-135">In hello results panel, select **CS Stars**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-cs-stars-tutorial/tutorial_csstars_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="53e56-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="53e56-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="53e56-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezést a CS csillag "Britta Simon." nevű tesztfelhasználó alapján</span><span class="sxs-lookup"><span data-stu-id="53e56-138">In this section, you configure and test Azure AD single sign-on with CS Stars based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="53e56-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó CS csillag tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="53e56-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in CS Stars is tooa user in Azure AD.</span></span> <span data-ttu-id="53e56-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello CS csillag közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="53e56-140">In other words, a link relationship between an Azure AD user and hello related user in CS Stars needs toobe established.</span></span>

<span data-ttu-id="53e56-141">CS csillag, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="53e56-141">In CS Stars, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="53e56-142">tooconfigure és az Azure AD az egyszeri bejelentkezés CS csillag-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="53e56-142">tooconfigure and test Azure AD single sign-on with CS Stars, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="53e56-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="53e56-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="53e56-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="53e56-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="53e56-145">**[CS csillag tesztfelhasználó létrehozása](#creating-a-cs-stars-test-user)**  -toohave egy Britta Simon az, hogy a felhasználó ábrázolása csatolt toohello az Azure AD CS csillag megfelelője.</span><span class="sxs-lookup"><span data-stu-id="53e56-145">**[Creating a CS Stars test user](#creating-a-cs-stars-test-user)** - toohave a counterpart of Britta Simon in CS Stars that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="53e56-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="53e56-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="53e56-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="53e56-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="53e56-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="53e56-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="53e56-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az CS csillag alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="53e56-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your CS Stars application.</span></span>

<span data-ttu-id="53e56-150">**az Azure AD tooconfigure egyszeri bejelentkezést a CS csillag, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="53e56-150">**tooconfigure Azure AD single sign-on with CS Stars, perform hello following steps:**</span></span>

1. <span data-ttu-id="53e56-151">Az Azure portál, a hello hello **CS csillag** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="53e56-151">In hello Azure portal, on hello **CS Stars** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="53e56-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="53e56-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cs-stars-tutorial/tutorial_csstars_samlbase.png)

3. <span data-ttu-id="53e56-155">A hello **CS csillag tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="53e56-155">On hello **CS Stars Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cs-stars-tutorial/tutorial_csstars_url.png)

    <span data-ttu-id="53e56-157">a.</span><span class="sxs-lookup"><span data-stu-id="53e56-157">a.</span></span> <span data-ttu-id="53e56-158">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<subdomain>.csstars.com/enterprise/default.cmdx?ssoclient=<uniqueid>`</span><span class="sxs-lookup"><span data-stu-id="53e56-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<subdomain>.csstars.com/enterprise/default.cmdx?ssoclient=<uniqueid>`</span></span>

    <span data-ttu-id="53e56-159">b.</span><span class="sxs-lookup"><span data-stu-id="53e56-159">b.</span></span> <span data-ttu-id="53e56-160">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<subdomain>.csstars.com/enterprise/`</span><span class="sxs-lookup"><span data-stu-id="53e56-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<subdomain>.csstars.com/enterprise/`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="53e56-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="53e56-161">These values are not real.</span></span> <span data-ttu-id="53e56-162">Frissítse a bejelentkezési URL-cím és azonosító a hello tényleges értékek.</span><span class="sxs-lookup"><span data-stu-id="53e56-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="53e56-163">Ügyfél [CS csillag ügyfél-támogatási csoport](http://www.marshclearsight.com/support/) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="53e56-163">Contact [CS Stars Client support team](http://www.marshclearsight.com/support/) tooget these values.</span></span> 
 
4. <span data-ttu-id="53e56-164">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="53e56-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cs-stars-tutorial/tutorial_csstars_certificate.png) 

5. <span data-ttu-id="53e56-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="53e56-166">Click **Save** button.</span></span>

    <span data-ttu-id="53e56-167">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cs-stars-tutorial/tutorial_general_400.png) 
<CS></span><span class="sxs-lookup"><span data-stu-id="53e56-167">![Configure Single Sign-On](./media/active-directory-saas-cs-stars-tutorial/tutorial_general_400.png) 
<CS></span></span>
6. <span data-ttu-id="53e56-168">tooconfigure egyszeri bejelentkezést a **CS csillag** oldalon kell letöltött toosend hello **metaadatainak XML-kódja** túl[CS csillag támogatási csoport](http://www.marshclearsight.com/support/).</span><span class="sxs-lookup"><span data-stu-id="53e56-168">tooconfigure single sign-on on **CS Stars** side, you need toosend hello downloaded **Metadata XML** too[CS Stars support team](http://www.marshclearsight.com/support/).</span></span> 
<CE>

> [!TIP]
> <span data-ttu-id="53e56-169">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="53e56-169">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="53e56-170">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="53e56-170">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="53e56-171">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="53e56-171">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="53e56-172">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="53e56-172">Creating an Azure AD test user</span></span>
<span data-ttu-id="53e56-173">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="53e56-173">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="53e56-175">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="53e56-175">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="53e56-176">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="53e56-176">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-cs-stars-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="53e56-178">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="53e56-178">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-cs-stars-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="53e56-180">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="53e56-180">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-cs-stars-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="53e56-182">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="53e56-182">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-cs-stars-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="53e56-184">a.</span><span class="sxs-lookup"><span data-stu-id="53e56-184">a.</span></span> <span data-ttu-id="53e56-185">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="53e56-185">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="53e56-186">b.</span><span class="sxs-lookup"><span data-stu-id="53e56-186">b.</span></span> <span data-ttu-id="53e56-187">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="53e56-187">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="53e56-188">c.</span><span class="sxs-lookup"><span data-stu-id="53e56-188">c.</span></span> <span data-ttu-id="53e56-189">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="53e56-189">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="53e56-190">d.</span><span class="sxs-lookup"><span data-stu-id="53e56-190">d.</span></span> <span data-ttu-id="53e56-191">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="53e56-191">Click **Create**.</span></span>
 
### <a name="creating-a-cs-stars-test-user"></a><span data-ttu-id="53e56-192">CS csillag tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="53e56-192">Creating a CS Stars test user</span></span>

<span data-ttu-id="53e56-193">hello ebben a szakaszban célja toocreate CS csillag Britta Simon nevű felhasználó.</span><span class="sxs-lookup"><span data-stu-id="53e56-193">hello objective of this section is toocreate a user called Britta Simon in CS Stars.</span></span>

<span data-ttu-id="53e56-194">toocontact szüksége tooget CS csillag létrehozott felhasználó, a [CS csillag támogatási csoport](http://www.marshclearsight.com/support/).</span><span class="sxs-lookup"><span data-stu-id="53e56-194">tooget a user created in CS Stars, you need toocontact your [CS Stars support team](http://www.marshclearsight.com/support/).</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="53e56-195">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="53e56-195">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="53e56-196">Ebben a szakaszban engedélyezése az Azure egyszeri bejelentkezés Britta Simon toouse tooCS csillag hozzáférés biztosítása.</span><span class="sxs-lookup"><span data-stu-id="53e56-196">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooCS Stars.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="53e56-198">**tooassign Britta Simon tooCS csillag, hajtsa végre hello a következő lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="53e56-198">**tooassign Britta Simon tooCS Stars, perform hello following steps:**</span></span>

1. <span data-ttu-id="53e56-199">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="53e56-199">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="53e56-201">Hello alkalmazások listában válassza ki a **CS csillag**.</span><span class="sxs-lookup"><span data-stu-id="53e56-201">In hello applications list, select **CS Stars**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cs-stars-tutorial/tutorial_csstars_app.png) 

3. <span data-ttu-id="53e56-203">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="53e56-203">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="53e56-205">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="53e56-205">Click **Add** button.</span></span> <span data-ttu-id="53e56-206">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="53e56-206">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="53e56-208">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="53e56-208">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="53e56-209">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="53e56-209">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="53e56-210">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="53e56-210">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="53e56-211">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="53e56-211">Testing single sign-on</span></span>

<span data-ttu-id="53e56-212">hello ebben a szakaszban célja tootest az egyszeri bejelentkezés konfigurációs használatával hello a hozzáférési Panel.</span><span class="sxs-lookup"><span data-stu-id="53e56-212">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>  
<span data-ttu-id="53e56-213">Hello CS csillag hello hozzáférési Panel csempére kattintva kapja meg automatikusan bejelentkezett tooyour CS csillag alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="53e56-213">When you click hello CS Stars tile in hello Access Panel, you should get automatically signed-on tooyour CS Stars application.</span></span>
 

## <a name="additional-resources"></a><span data-ttu-id="53e56-214">További források</span><span class="sxs-lookup"><span data-stu-id="53e56-214">Additional resources</span></span>

* [<span data-ttu-id="53e56-215">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="53e56-215">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="53e56-216">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="53e56-216">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_203.png

