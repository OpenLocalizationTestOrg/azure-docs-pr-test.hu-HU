---
title: "Oktatóanyag: Azure Active Directoryval integrált SimpleNexus |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és SimpleNexus között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 89821a05-88e2-4579-b144-0123b2b9cb95
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: 89f455d8c551045ddfcbe7234e86b13dad1140a1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-simplenexus"></a><span data-ttu-id="0a657-103">Oktatóanyag: Azure Active Directoryval integrált SimpleNexus</span><span class="sxs-lookup"><span data-stu-id="0a657-103">Tutorial: Azure Active Directory integration with SimpleNexus</span></span>

<span data-ttu-id="0a657-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate SimpleNexus az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="0a657-104">In this tutorial, you learn how toointegrate SimpleNexus with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="0a657-105">SimpleNexus integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="0a657-105">Integrating SimpleNexus with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="0a657-106">Megadhatja a hozzáférés tooSimpleNexus rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="0a657-106">You can control in Azure AD who has access tooSimpleNexus</span></span>
- <span data-ttu-id="0a657-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooSimpleNexus (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="0a657-107">You can enable your users tooautomatically get signed-on tooSimpleNexus (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="0a657-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="0a657-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="0a657-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="0a657-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0a657-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="0a657-110">Prerequisites</span></span>

<span data-ttu-id="0a657-111">az Azure AD integrálása SimpleNexus tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="0a657-111">tooconfigure Azure AD integration with SimpleNexus, you need hello following items:</span></span>

- <span data-ttu-id="0a657-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="0a657-112">An Azure AD subscription</span></span>
- <span data-ttu-id="0a657-113">Egy SimpleNexus egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="0a657-113">A SimpleNexus single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="0a657-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="0a657-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="0a657-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="0a657-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="0a657-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="0a657-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="0a657-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="0a657-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="0a657-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="0a657-118">Scenario description</span></span>
<span data-ttu-id="0a657-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="0a657-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="0a657-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="0a657-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="0a657-121">Hello gyűjteményből SimpleNexus hozzáadása</span><span class="sxs-lookup"><span data-stu-id="0a657-121">Adding SimpleNexus from hello gallery</span></span>
2. <span data-ttu-id="0a657-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="0a657-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-simplenexus-from-hello-gallery"></a><span data-ttu-id="0a657-123">Hello gyűjteményből SimpleNexus hozzáadása</span><span class="sxs-lookup"><span data-stu-id="0a657-123">Adding SimpleNexus from hello gallery</span></span>
<span data-ttu-id="0a657-124">tooconfigure hello integrációja SimpleNexus az Azure AD-be, meg kell tooadd SimpleNexus hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="0a657-124">tooconfigure hello integration of SimpleNexus into Azure AD, you need tooadd SimpleNexus from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="0a657-125">**tooadd SimpleNexus hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="0a657-125">**tooadd SimpleNexus from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="0a657-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="0a657-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="0a657-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="0a657-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="0a657-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="0a657-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="0a657-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="0a657-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="0a657-133">Hello keresési mezőbe, írja be a **SimpleNexus**.</span><span class="sxs-lookup"><span data-stu-id="0a657-133">In hello search box, type **SimpleNexus**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-simplenexus-tutorial/tutorial_simplenexus_search.png)

5. <span data-ttu-id="0a657-135">A hello eredmények panelen válassza ki a **SimpleNexus**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="0a657-135">In hello results panel, select **SimpleNexus**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-simplenexus-tutorial/tutorial_simplenexus_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="0a657-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="0a657-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="0a657-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján SimpleNexus.</span><span class="sxs-lookup"><span data-stu-id="0a657-138">In this section, you configure and test Azure AD single sign-on with SimpleNexus based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="0a657-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó SimpleNexus tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="0a657-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in SimpleNexus is tooa user in Azure AD.</span></span> <span data-ttu-id="0a657-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello SimpleNexus közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="0a657-140">In other words, a link relationship between an Azure AD user and hello related user in SimpleNexus needs toobe established.</span></span>

<span data-ttu-id="0a657-141">SimpleNexus, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="0a657-141">In SimpleNexus, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="0a657-142">tooconfigure és az Azure AD az egyszeri bejelentkezés SimpleNexus-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="0a657-142">tooconfigure and test Azure AD single sign-on with SimpleNexus, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="0a657-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="0a657-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="0a657-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="0a657-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="0a657-145">**[SimpleNexus tesztfelhasználó létrehozása](#creating-a-simplenexus-test-user)**  -toohave egy megfelelője a Britta Simon a SimpleNexus, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="0a657-145">**[Creating a SimpleNexus test user](#creating-a-simplenexus-test-user)** - toohave a counterpart of Britta Simon in SimpleNexus that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="0a657-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="0a657-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="0a657-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="0a657-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="0a657-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="0a657-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="0a657-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az SimpleNexus alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="0a657-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your SimpleNexus application.</span></span>

<span data-ttu-id="0a657-150">**az Azure AD tooconfigure egyszeri bejelentkezést a SimpleNexus, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="0a657-150">**tooconfigure Azure AD single sign-on with SimpleNexus, perform hello following steps:**</span></span>

1. <span data-ttu-id="0a657-151">Az Azure portál, a hello hello **SimpleNexus** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="0a657-151">In hello Azure portal, on hello **SimpleNexus** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="0a657-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="0a657-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-simplenexus-tutorial/tutorial_simplenexus_samlbase.png)

3. <span data-ttu-id="0a657-155">A hello **SimpleNexus tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="0a657-155">On hello **SimpleNexus Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-simplenexus-tutorial/tutorial_simplenexus_url.png)

    <span data-ttu-id="0a657-157">a.</span><span class="sxs-lookup"><span data-stu-id="0a657-157">a.</span></span> <span data-ttu-id="0a657-158">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://simplenexus.com/<companyname>_login`</span><span class="sxs-lookup"><span data-stu-id="0a657-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://simplenexus.com/<companyname>_login`</span></span>

    <span data-ttu-id="0a657-159">b.</span><span class="sxs-lookup"><span data-stu-id="0a657-159">b.</span></span> <span data-ttu-id="0a657-160">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://simplenexus.com/<companyname>`</span><span class="sxs-lookup"><span data-stu-id="0a657-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://simplenexus.com/<companyname>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="0a657-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="0a657-161">These values are not real.</span></span> <span data-ttu-id="0a657-162">Frissítse a bejelentkezési URL-cím és azonosító a hello tényleges értékek.</span><span class="sxs-lookup"><span data-stu-id="0a657-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="0a657-163">Ügyfél [SimpleNexus ügyfél-támogatási csoport](https://simplenexus.com/site/contact) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="0a657-163">Contact [SimpleNexus Client support team](https://simplenexus.com/site/contact) tooget these values.</span></span> 
 
4. <span data-ttu-id="0a657-164">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="0a657-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-simplenexus-tutorial/tutorial_simplenexus_certificate.png) 

5. <span data-ttu-id="0a657-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="0a657-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-simplenexus-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="0a657-168">tooconfigure egyszeri bejelentkezést a **SimpleNexus** oldalon kell letöltött toosend hello **metaadatainak XML-kódja** túl[SimpleNexus támogatási csoport](https://simplenexus.com/site/contact).</span><span class="sxs-lookup"><span data-stu-id="0a657-168">tooconfigure single sign-on on **SimpleNexus** side, you need toosend hello downloaded **Metadata XML** too[SimpleNexus support team](https://simplenexus.com/site/contact).</span></span> <span data-ttu-id="0a657-169">Maguk állítják be ezt a beállítást toohave hello SAML SSO kapcsolat mindkét oldalán megfelelően beállítva.</span><span class="sxs-lookup"><span data-stu-id="0a657-169">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="0a657-170">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="0a657-170">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="0a657-171">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="0a657-171">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="0a657-172">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="0a657-172">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="0a657-173">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="0a657-173">Creating an Azure AD test user</span></span>
<span data-ttu-id="0a657-174">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="0a657-174">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="0a657-176">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="0a657-176">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="0a657-177">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="0a657-177">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-simplenexus-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="0a657-179">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="0a657-179">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-simplenexus-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="0a657-181">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="0a657-181">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-simplenexus-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="0a657-183">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="0a657-183">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-simplenexus-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="0a657-185">a.</span><span class="sxs-lookup"><span data-stu-id="0a657-185">a.</span></span> <span data-ttu-id="0a657-186">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="0a657-186">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="0a657-187">b.</span><span class="sxs-lookup"><span data-stu-id="0a657-187">b.</span></span> <span data-ttu-id="0a657-188">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="0a657-188">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="0a657-189">c.</span><span class="sxs-lookup"><span data-stu-id="0a657-189">c.</span></span> <span data-ttu-id="0a657-190">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="0a657-190">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="0a657-191">d.</span><span class="sxs-lookup"><span data-stu-id="0a657-191">d.</span></span> <span data-ttu-id="0a657-192">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="0a657-192">Click **Create**.</span></span>
 
### <a name="creating-a-simplenexus-test-user"></a><span data-ttu-id="0a657-193">SimpleNexus tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="0a657-193">Creating a SimpleNexus test user</span></span>

<span data-ttu-id="0a657-194">A sorrend tooenable az Azure AD felhasználók toolog a tooSimpleNexus azok ki kell építenie SimpleNexus be.</span><span class="sxs-lookup"><span data-stu-id="0a657-194">In order tooenable Azure AD users toolog in tooSimpleNexus, they must be provisioned into SimpleNexus.</span></span>

<span data-ttu-id="0a657-195">Kis-és SimpleNexus kiépítés hello hello Bérlői rendszergazda által végzett manuális feladat.</span><span class="sxs-lookup"><span data-stu-id="0a657-195">In hello case of SimpleNexus, provisioning is a manual task performed by hello tenant administrator.</span></span>

>[!NOTE]
><span data-ttu-id="0a657-196">Bármely más SimpleNexus felhasználói fiók létrehozása eszközök vagy SimpleNexus tooprovision által nyújtott API-k AAD felhasználói fiókokat.</span><span class="sxs-lookup"><span data-stu-id="0a657-196">You can use any other SimpleNexus user account creation tools or APIs provided by SimpleNexus tooprovision AAD user accounts.</span></span> 
> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="0a657-197">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="0a657-197">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="0a657-198">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooSimpleNexus megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="0a657-198">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooSimpleNexus.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="0a657-200">**tooassign Britta Simon tooSimpleNexus, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="0a657-200">**tooassign Britta Simon tooSimpleNexus, perform hello following steps:**</span></span>

1. <span data-ttu-id="0a657-201">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="0a657-201">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="0a657-203">Hello alkalmazások listában válassza ki a **SimpleNexus**.</span><span class="sxs-lookup"><span data-stu-id="0a657-203">In hello applications list, select **SimpleNexus**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-simplenexus-tutorial/tutorial_simplenexus_app.png) 

3. <span data-ttu-id="0a657-205">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="0a657-205">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="0a657-207">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="0a657-207">Click **Add** button.</span></span> <span data-ttu-id="0a657-208">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="0a657-208">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="0a657-210">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="0a657-210">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="0a657-211">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="0a657-211">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="0a657-212">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="0a657-212">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="0a657-213">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="0a657-213">Testing single sign-on</span></span>

<span data-ttu-id="0a657-214">hello ebben a szakaszban célja tootest az egyszeri bejelentkezés konfigurációs használatával hello a hozzáférési Panel.</span><span class="sxs-lookup"><span data-stu-id="0a657-214">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="0a657-215">Hello SimpleNexus hello hozzáférési Panel csempére kattintva kapja meg automatikusan bejelentkezett tooyour SimpleNexus alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="0a657-215">When you click hello SimpleNexus tile in hello Access Panel, you should get automatically signed-on tooyour SimpleNexus application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0a657-216">További források</span><span class="sxs-lookup"><span data-stu-id="0a657-216">Additional resources</span></span>

* [<span data-ttu-id="0a657-217">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="0a657-217">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="0a657-218">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="0a657-218">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-simplenexus-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-simplenexus-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-simplenexus-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-simplenexus-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-simplenexus-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-simplenexus-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-simplenexus-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-simplenexus-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-simplenexus-tutorial/tutorial_general_203.png

