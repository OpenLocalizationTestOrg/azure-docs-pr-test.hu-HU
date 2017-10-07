---
title: "Oktatóanyag: Azure Active Directoryval integrált itslearning |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és itslearning között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 60587ba3-1396-4b8a-9ac1-e22a98e5e0ac
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/17/2017
ms.author: jeedes
ms.openlocfilehash: 4ee6c8d450cc3972a87da67fc79890473cfa498a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-itslearning"></a><span data-ttu-id="86a85-103">Oktatóanyag: Azure Active Directoryval integrált itslearning</span><span class="sxs-lookup"><span data-stu-id="86a85-103">Tutorial: Azure Active Directory integration with itslearning</span></span>

<span data-ttu-id="86a85-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate itslearning az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="86a85-104">In this tutorial, you learn how toointegrate itslearning with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="86a85-105">Itslearning integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="86a85-105">Integrating itslearning with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="86a85-106">Megadhatja a hozzáférés tooitslearning rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="86a85-106">You can control in Azure AD who has access tooitslearning</span></span>
- <span data-ttu-id="86a85-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooitslearning (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="86a85-107">You can enable your users tooautomatically get signed-on tooitslearning (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="86a85-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="86a85-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="86a85-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="86a85-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="86a85-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="86a85-110">Prerequisites</span></span>

<span data-ttu-id="86a85-111">az Azure AD integrálása itslearning tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="86a85-111">tooconfigure Azure AD integration with itslearning, you need hello following items:</span></span>

- <span data-ttu-id="86a85-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="86a85-112">An Azure AD subscription</span></span>
- <span data-ttu-id="86a85-113">Egy itslearning egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="86a85-113">An itslearning single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="86a85-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="86a85-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="86a85-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="86a85-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="86a85-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="86a85-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="86a85-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="86a85-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="86a85-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="86a85-118">Scenario description</span></span>
<span data-ttu-id="86a85-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="86a85-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="86a85-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="86a85-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="86a85-121">Hello gyűjteményből itslearning hozzáadása</span><span class="sxs-lookup"><span data-stu-id="86a85-121">Adding itslearning from hello gallery</span></span>
2. <span data-ttu-id="86a85-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="86a85-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-itslearning-from-hello-gallery"></a><span data-ttu-id="86a85-123">Hello gyűjteményből itslearning hozzáadása</span><span class="sxs-lookup"><span data-stu-id="86a85-123">Adding itslearning from hello gallery</span></span>
<span data-ttu-id="86a85-124">tooconfigure hello integrációja itslearning az Azure AD-be, meg kell tooadd itslearning hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="86a85-124">tooconfigure hello integration of itslearning into Azure AD, you need tooadd itslearning from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="86a85-125">**tooadd itslearning hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="86a85-125">**tooadd itslearning from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="86a85-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="86a85-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="86a85-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="86a85-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="86a85-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="86a85-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="86a85-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="86a85-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="86a85-133">Hello keresési mezőbe, írja be a **itslearning**.</span><span class="sxs-lookup"><span data-stu-id="86a85-133">In hello search box, type **itslearning**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-itslearning-tutorial/tutorial_itslearning_search.png)

5. <span data-ttu-id="86a85-135">A hello eredmények panelen válassza ki a **itslearning**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="86a85-135">In hello results panel, select **itslearning**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-itslearning-tutorial/tutorial_itslearning_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="86a85-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="86a85-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="86a85-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján itslearning.</span><span class="sxs-lookup"><span data-stu-id="86a85-138">In this section, you configure and test Azure AD single sign-on with itslearning based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="86a85-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó itslearning tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="86a85-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in itslearning is tooa user in Azure AD.</span></span> <span data-ttu-id="86a85-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello itslearning közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="86a85-140">In other words, a link relationship between an Azure AD user and hello related user in itslearning needs toobe established.</span></span>

<span data-ttu-id="86a85-141">Itslearning, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="86a85-141">In itslearning, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="86a85-142">tooconfigure és az Azure AD az egyszeri bejelentkezés itslearning-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="86a85-142">tooconfigure and test Azure AD single sign-on with itslearning, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="86a85-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="86a85-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="86a85-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="86a85-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="86a85-145">**[Egy itslearning tesztfelhasználó létrehozása](#creating-an-itslearning-test-user)**  -toohave Britta Simon egy partner, a itslearning, amely a felhasználó csatolt toohello az Azure AD-ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="86a85-145">**[Creating an itslearning test user](#creating-an-itslearning-test-user)** - toohave a counterpart of Britta Simon in itslearning that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="86a85-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="86a85-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="86a85-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="86a85-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="86a85-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="86a85-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="86a85-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az itslearning alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="86a85-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your itslearning application.</span></span>

<span data-ttu-id="86a85-150">**az Azure AD tooconfigure egyszeri bejelentkezést a itslearning, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="86a85-150">**tooconfigure Azure AD single sign-on with itslearning, perform hello following steps:**</span></span>

1. <span data-ttu-id="86a85-151">Az Azure portál, a hello hello **itslearning** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="86a85-151">In hello Azure portal, on hello **itslearning** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="86a85-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="86a85-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-itslearning-tutorial/tutorial_itslearning_samlbase.png)

3. <span data-ttu-id="86a85-155">A hello **itslearning tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="86a85-155">On hello **itslearning Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-itslearning-tutorial/tutorial_itslearning_url.png)

    <span data-ttu-id="86a85-157">a.</span><span class="sxs-lookup"><span data-stu-id="86a85-157">a.</span></span> <span data-ttu-id="86a85-158">A hello **bejelentkezési URL-cím** szövegmező, adja meg az URL-címet:</span><span class="sxs-lookup"><span data-stu-id="86a85-158">In hello **Sign-on URL** textbox, type a URL as:</span></span>
    | |
    |--| 
    | `https://www.itslearning.com/index.aspx`|
    | `https://us1.itslearning.com/index.aspx`|

    <span data-ttu-id="86a85-159">b.</span><span class="sxs-lookup"><span data-stu-id="86a85-159">b.</span></span> <span data-ttu-id="86a85-160">A hello **azonosító** szövegmező, adja meg az URL-címet:`urn:mace:saml2v2.no:services:com.itslearning`</span><span class="sxs-lookup"><span data-stu-id="86a85-160">In hello **Identifier** textbox, type a URL as: `urn:mace:saml2v2.no:services:com.itslearning`</span></span>

4. <span data-ttu-id="86a85-161">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="86a85-161">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-itslearning-tutorial/tutorial_itslearning_certificate.png) 

5. <span data-ttu-id="86a85-163">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="86a85-163">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-itslearning-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="86a85-165">tooconfigure egyszeri bejelentkezést a **itslearning** oldalon kell letöltött toosend hello **metaadatainak XML-kódja** túl[itslearning támogatási csoport](mailto:support@itslearning.com).</span><span class="sxs-lookup"><span data-stu-id="86a85-165">tooconfigure single sign-on on **itslearning** side, you need toosend hello downloaded **Metadata XML** too[itslearning support team](mailto:support@itslearning.com).</span></span> <span data-ttu-id="86a85-166">Maguk állítják be ezt a beállítást toohave hello SAML SSO kapcsolat mindkét oldalán megfelelően beállítva.</span><span class="sxs-lookup"><span data-stu-id="86a85-166">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="86a85-167">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="86a85-167">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="86a85-168">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="86a85-168">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="86a85-169">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="86a85-169">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="86a85-170">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="86a85-170">Creating an Azure AD test user</span></span>
<span data-ttu-id="86a85-171">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="86a85-171">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="86a85-173">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="86a85-173">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="86a85-174">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="86a85-174">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-itslearning-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="86a85-176">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="86a85-176">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-itslearning-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="86a85-178">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="86a85-178">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-itslearning-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="86a85-180">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="86a85-180">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-itslearning-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="86a85-182">a.</span><span class="sxs-lookup"><span data-stu-id="86a85-182">a.</span></span> <span data-ttu-id="86a85-183">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="86a85-183">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="86a85-184">b.</span><span class="sxs-lookup"><span data-stu-id="86a85-184">b.</span></span> <span data-ttu-id="86a85-185">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="86a85-185">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="86a85-186">c.</span><span class="sxs-lookup"><span data-stu-id="86a85-186">c.</span></span> <span data-ttu-id="86a85-187">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="86a85-187">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="86a85-188">d.</span><span class="sxs-lookup"><span data-stu-id="86a85-188">d.</span></span> <span data-ttu-id="86a85-189">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="86a85-189">Click **Create**.</span></span>
 
### <a name="creating-an-itslearning-test-user"></a><span data-ttu-id="86a85-190">Egy itslearning tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="86a85-190">Creating an itslearning test user</span></span>

<span data-ttu-id="86a85-191">Ebben a szakaszban egy itslearning Britta Simon nevű felhasználót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="86a85-191">In this section, you create a user called Britta Simon in itslearning.</span></span> <span data-ttu-id="86a85-192">Együttműködve [itslearning ügyfél-támogatási csoport](mailto:support@itslearning.com) felhasználót is hozzáadhat hello hello itslearning platform.</span><span class="sxs-lookup"><span data-stu-id="86a85-192">Work with [itslearning Client support team](mailto:support@itslearning.com) to add hello users in hello itslearning platform.</span></span> <span data-ttu-id="86a85-193">Felhasználók kell létrehoznia és aktiválni az egyszeri bejelentkezés használata előtt.</span><span class="sxs-lookup"><span data-stu-id="86a85-193">Users must be created and activated before you use single sign-on.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="86a85-194">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="86a85-194">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="86a85-195">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooitslearning megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="86a85-195">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooitslearning.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="86a85-197">**tooassign Britta Simon tooitslearning, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="86a85-197">**tooassign Britta Simon tooitslearning, perform hello following steps:**</span></span>

1. <span data-ttu-id="86a85-198">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="86a85-198">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="86a85-200">Hello alkalmazások listában válassza ki a **itslearning**.</span><span class="sxs-lookup"><span data-stu-id="86a85-200">In hello applications list, select **itslearning**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-itslearning-tutorial/tutorial_itslearning_app.png) 

3. <span data-ttu-id="86a85-202">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="86a85-202">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="86a85-204">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="86a85-204">Click **Add** button.</span></span> <span data-ttu-id="86a85-205">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="86a85-205">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="86a85-207">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="86a85-207">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="86a85-208">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="86a85-208">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="86a85-209">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="86a85-209">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="86a85-210">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="86a85-210">Testing single sign-on</span></span>

<span data-ttu-id="86a85-211">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="86a85-211">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="86a85-212">Ha a hozzáférési Panel hello hello itslearning csempe gombra kattint, itslearning alkalmazás bejelentkezési oldalán szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="86a85-212">When you click hello itslearning tile in hello Access Panel, you should get login page of itslearning application.</span></span> <span data-ttu-id="86a85-213">Kattintson a **jelentkezzen be a Windows Azure ACS1** sikeres bejelentkezés hello alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="86a85-213">Click **Log in with Windows Azure ACS1** for successful login into hello application.</span></span>

  ![Bejelentkezés](./media/active-directory-saas-itslearning-tutorial/login.png)

<span data-ttu-id="86a85-215">További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="86a85-215">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="86a85-216">További források</span><span class="sxs-lookup"><span data-stu-id="86a85-216">Additional resources</span></span>

* [<span data-ttu-id="86a85-217">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="86a85-217">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="86a85-218">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="86a85-218">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-itslearning-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-itslearning-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-itslearning-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-itslearning-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-itslearning-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-itslearning-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-itslearning-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-itslearning-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-itslearning-tutorial/tutorial_general_203.png

