---
title: "Oktatóanyag: Azure Active Directoryval integrált Inkling |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Inkling között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 64c7ee45-ee8a-42f7-bf04-fd0e00833ea9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/06/2017
ms.author: jeedes
ms.openlocfilehash: 544101f1972ec16222406b761d2b6f4987458df5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-inkling"></a><span data-ttu-id="78d1e-103">Oktatóanyag: Azure Active Directoryval integrált Inkling</span><span class="sxs-lookup"><span data-stu-id="78d1e-103">Tutorial: Azure Active Directory integration with Inkling</span></span>

<span data-ttu-id="78d1e-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Inkling az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="78d1e-104">In this tutorial, you learn how toointegrate Inkling with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="78d1e-105">Inkling integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="78d1e-105">Integrating Inkling with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="78d1e-106">Megadhatja a hozzáférés tooInkling rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="78d1e-106">You can control in Azure AD who has access tooInkling</span></span>
- <span data-ttu-id="78d1e-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooInkling (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="78d1e-107">You can enable your users tooautomatically get signed-on tooInkling (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="78d1e-108">Kezelheti a fiókokat, egy központi helyen - hello Azure felügyeleti portálon</span><span class="sxs-lookup"><span data-stu-id="78d1e-108">You can manage your accounts in one central location - hello Azure Management portal</span></span>

<span data-ttu-id="78d1e-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="78d1e-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="78d1e-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="78d1e-110">Prerequisites</span></span>

<span data-ttu-id="78d1e-111">az Azure AD integrálása Inkling tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="78d1e-111">tooconfigure Azure AD integration with Inkling, you need hello following items:</span></span>

- <span data-ttu-id="78d1e-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="78d1e-112">An Azure AD subscription</span></span>
- <span data-ttu-id="78d1e-113">Egy Inkling egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="78d1e-113">An Inkling single-sign on enabled subscription</span></span>


> [!NOTE]
> <span data-ttu-id="78d1e-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="78d1e-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>


<span data-ttu-id="78d1e-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="78d1e-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="78d1e-116">Ne használja az éles környezetben, ha ez nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="78d1e-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="78d1e-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="78d1e-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>


## <a name="scenario-description"></a><span data-ttu-id="78d1e-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="78d1e-118">Scenario description</span></span>
<span data-ttu-id="78d1e-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="78d1e-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="78d1e-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="78d1e-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="78d1e-121">Hello gyűjteményből Inkling hozzáadása</span><span class="sxs-lookup"><span data-stu-id="78d1e-121">Adding Inkling from hello gallery</span></span>
2. <span data-ttu-id="78d1e-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="78d1e-122">Configuring and testing Azure AD single sign-on</span></span>


## <a name="adding-inkling-from-hello-gallery"></a><span data-ttu-id="78d1e-123">Hello gyűjteményből Inkling hozzáadása</span><span class="sxs-lookup"><span data-stu-id="78d1e-123">Adding Inkling from hello gallery</span></span>
<span data-ttu-id="78d1e-124">tooconfigure hello integrációja Inkling az Azure AD-be, meg kell tooadd Inkling hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="78d1e-124">tooconfigure hello integration of Inkling into Azure AD, you need tooadd Inkling from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="78d1e-125">**tooadd Inkling hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="78d1e-125">**tooadd Inkling from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="78d1e-126">A hello  **[Azure felügyeleti portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="78d1e-126">In hello **[Azure Management Portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="78d1e-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="78d1e-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="78d1e-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="78d1e-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="78d1e-131">Kattintson a **Hozzáadás** hello párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="78d1e-131">Click **Add** button on hello top of hello dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="78d1e-133">Hello keresési mezőbe, írja be a **Inkling**.</span><span class="sxs-lookup"><span data-stu-id="78d1e-133">In hello search box, type **Inkling**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-inkling-tutorial/tutorial_inkling_001.png)

5. <span data-ttu-id="78d1e-135">A hello eredmények panelen válassza ki a **Inkling**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="78d1e-135">In hello results panel, select **Inkling**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-inkling-tutorial/tutorial_inkling_0001.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="78d1e-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="78d1e-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="78d1e-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Inkling.</span><span class="sxs-lookup"><span data-stu-id="78d1e-138">In this section, you configure and test Azure AD single sign-on with Inkling based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="78d1e-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Inkling tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="78d1e-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Inkling is tooa user in Azure AD.</span></span> <span data-ttu-id="78d1e-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Inkling közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="78d1e-140">In other words, a link relationship between an Azure AD user and hello related user in Inkling needs toobe established.</span></span>

<span data-ttu-id="78d1e-141">Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** a Inkling.</span><span class="sxs-lookup"><span data-stu-id="78d1e-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Inkling.</span></span>

<span data-ttu-id="78d1e-142">tooconfigure és az Azure AD az egyszeri bejelentkezés Inkling-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="78d1e-142">tooconfigure and test Azure AD single sign-on with Inkling, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="78d1e-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="78d1e-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="78d1e-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="78d1e-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="78d1e-145">**[Egy Inkling tesztfelhasználó létrehozása](#creating-an-inkling-test-user)**  -toohave Britta Simon Inkling, amely az Azure AD csatolt toohello ábrázolása rá, hogy az egy megfelelője.</span><span class="sxs-lookup"><span data-stu-id="78d1e-145">**[Creating an Inkling test user](#creating-an-inkling-test-user)** - toohave a counterpart of Britta Simon in Inkling that is linked toohello Azure AD representation of her.</span></span>
4. <span data-ttu-id="78d1e-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="78d1e-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="78d1e-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="78d1e-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="78d1e-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="78d1e-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="78d1e-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel hello Azure felügyeleti portálon, és konfigurálása egyszeri bejelentkezéshez az Inkling alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="78d1e-149">In this section, you enable Azure AD single sign-on in hello Azure Management portal and configure single sign-on in your Inkling application.</span></span>

<span data-ttu-id="78d1e-150">**az Azure AD tooconfigure egyszeri bejelentkezést a Inkling, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="78d1e-150">**tooconfigure Azure AD single sign-on with Inkling, perform hello following steps:**</span></span>

1. <span data-ttu-id="78d1e-151">Hello Azure felügyeleti portálon, a hello **Inkling** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="78d1e-151">In hello Azure Management portal, on hello **Inkling** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="78d1e-153">A hello **egyszeri bejelentkezés** párbeszédpanel, mint **mód** kiválasztása **SAML-alapú bejelentkezés** tooenable az egyszeri bejelentkezés.</span><span class="sxs-lookup"><span data-stu-id="78d1e-153">On hello **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** tooenable single sign on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-inkling-tutorial/tutorial_general_300.png)
    
3. <span data-ttu-id="78d1e-155">A hello **Inkling tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="78d1e-155">On hello **Inkling Domain and URLs** section, perform hello following steps:</span></span>
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-inkling-tutorial/tutorial_inkling_01.png)

    <span data-ttu-id="78d1e-157">a.</span><span class="sxs-lookup"><span data-stu-id="78d1e-157">a.</span></span> <span data-ttu-id="78d1e-158">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://api.inkling.com/saml/v2/metadata/<user-id>`</span><span class="sxs-lookup"><span data-stu-id="78d1e-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://api.inkling.com/saml/v2/metadata/<user-id>`</span></span>

    <span data-ttu-id="78d1e-159">b.</span><span class="sxs-lookup"><span data-stu-id="78d1e-159">b.</span></span> <span data-ttu-id="78d1e-160">A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://api.inkling.com/saml/v2/acs/<user-id>`</span><span class="sxs-lookup"><span data-stu-id="78d1e-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://api.inkling.com/saml/v2/acs/<user-id>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="78d1e-161">Ne feledje, hogy ezek nincsenek hello valódi értékek.</span><span class="sxs-lookup"><span data-stu-id="78d1e-161">Please note that these are not hello real values.</span></span> <span data-ttu-id="78d1e-162">Tooupdate rendelkezik ezekkel az értékekkel rendelkező hello tényleges azonosítója és válasz URL-címet.</span><span class="sxs-lookup"><span data-stu-id="78d1e-162">You have tooupdate these values with hello actual Identifier and Reply URL.</span></span> <span data-ttu-id="78d1e-163">Ügyfél [Inkling támogatási csoport](mailto:press@inkling.com) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="78d1e-163">Contact [Inkling support team](mailto:press@inkling.com) tooget these values.</span></span>

4. <span data-ttu-id="78d1e-164">A hello **SAML-aláíró tanúsítványa** kattintson **hozzon létre új tanúsítvány**.</span><span class="sxs-lookup"><span data-stu-id="78d1e-164">On hello **SAML Signing Certificate** section, click **Create new certificate**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-inkling-tutorial/tutorial_general_400.png)    

5. <span data-ttu-id="78d1e-166">A hello **új tanúsítvány létrehozása** párbeszédpanelen hello naptár ikonra, és válassza ki az **lejárati dátum**.</span><span class="sxs-lookup"><span data-stu-id="78d1e-166">On hello **Create New Certificate** dialog, click hello calendar icon and select an **expiry date**.</span></span> <span data-ttu-id="78d1e-167">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="78d1e-167">Then click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-inkling-tutorial/tutorial_general_500.png)

6. <span data-ttu-id="78d1e-169">A hello **SAML-aláíró tanúsítványa** szakaszban jelölje be **új tanúsítvány aktiválásához** kattintson **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="78d1e-169">On hello **SAML Signing Certificate** section, select **Make new certificate active** and click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-inkling-tutorial/tutorial_inkling_02.png)

7. <span data-ttu-id="78d1e-171">A hello előugró ablak **helyettesítő tanúsítvány** ablak, kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="78d1e-171">On hello pop-up **Rollover certificate** window, click **OK**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-inkling-tutorial/tutorial_general_600.png)

8. <span data-ttu-id="78d1e-173">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="78d1e-173">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-inkling-tutorial/tutorial_inkling_03.png) 

9. <span data-ttu-id="78d1e-175">az alkalmazáshoz konfigurált SSO tooget, forduljon a [Inkling támogatási csoport](mailto:press@inkling.com) és adja meg a letöltött **metaadatok**.</span><span class="sxs-lookup"><span data-stu-id="78d1e-175">tooget SSO configured for your application, contact [Inkling support team](mailto:press@inkling.com) and provide them with downloaded **metadata**.</span></span> 


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="78d1e-176">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="78d1e-176">Creating an Azure AD test user</span></span>
<span data-ttu-id="78d1e-177">hello ebben a szakaszban célja toocreate tesztfelhasználó Britta Simon nevű hello Azure felügyeleti portálon.</span><span class="sxs-lookup"><span data-stu-id="78d1e-177">hello objective of this section is toocreate a test user in hello Azure Management portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="78d1e-179">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="78d1e-179">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="78d1e-180">A hello **Azure Management portal**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="78d1e-180">In hello **Azure Management portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-inkling-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="78d1e-182">Nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó** toodisplay hello azoknak a felhasználóknak.</span><span class="sxs-lookup"><span data-stu-id="78d1e-182">Go too**Users and groups** and click **All users** toodisplay hello list of users.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-inkling-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="78d1e-184">Hello párbeszédpanel hello tetején kattintson **Hozzáadás** tooopen hello **felhasználói** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="78d1e-184">At hello top of hello dialog click **Add** tooopen hello **User** dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-inkling-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="78d1e-186">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="78d1e-186">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-inkling-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="78d1e-188">a.</span><span class="sxs-lookup"><span data-stu-id="78d1e-188">a.</span></span> <span data-ttu-id="78d1e-189">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="78d1e-189">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="78d1e-190">b.</span><span class="sxs-lookup"><span data-stu-id="78d1e-190">b.</span></span> <span data-ttu-id="78d1e-191">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="78d1e-191">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="78d1e-192">c.</span><span class="sxs-lookup"><span data-stu-id="78d1e-192">c.</span></span> <span data-ttu-id="78d1e-193">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="78d1e-193">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="78d1e-194">d.</span><span class="sxs-lookup"><span data-stu-id="78d1e-194">d.</span></span> <span data-ttu-id="78d1e-195">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="78d1e-195">Click **Create**.</span></span> 



### <a name="creating-an-inkling-test-user"></a><span data-ttu-id="78d1e-196">Egy Inkling tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="78d1e-196">Creating an Inkling test user</span></span>

<span data-ttu-id="78d1e-197">Ebben a szakaszban egy Inkling Britta Simon nevű felhasználót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="78d1e-197">In this section, you create a user called Britta Simon in Inkling.</span></span> <span data-ttu-id="78d1e-198">Adjon együttműködve [Inkling támogatási csoport](mailto:press@inkling.com) tooadd hello felhasználók hello Inkling platform.</span><span class="sxs-lookup"><span data-stu-id="78d1e-198">Please work with [Inkling support team](mailto:press@inkling.com) tooadd hello users in hello Inkling platform.</span></span>


### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="78d1e-199">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="78d1e-199">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="78d1e-200">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés saját hozzáférés tooInkling megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="78d1e-200">In this section, you enable Britta Simon toouse Azure single sign-on by granting her access tooInkling.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="78d1e-202">**tooassign Britta Simon tooInkling, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="78d1e-202">**tooassign Britta Simon tooInkling, perform hello following steps:**</span></span>

1. <span data-ttu-id="78d1e-203">Hello Azure felügyeleti portálján, nyissa meg a hello alkalmazások megtekintése, majd toohello könyvtár nézetben keresse meg, és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="78d1e-203">In hello Azure Management portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="78d1e-205">Hello alkalmazások listában válassza ki a **Inkling**.</span><span class="sxs-lookup"><span data-stu-id="78d1e-205">In hello applications list, select **Inkling**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-inkling-tutorial/tutorial_inkling_50.png) 

3. <span data-ttu-id="78d1e-207">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="78d1e-207">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="78d1e-209">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="78d1e-209">Click **Add** button.</span></span> <span data-ttu-id="78d1e-210">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="78d1e-210">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="78d1e-212">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="78d1e-212">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="78d1e-213">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="78d1e-213">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="78d1e-214">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="78d1e-214">Click **Assign** button on **Add Assignment** dialog.</span></span>
    


### <a name="testing-single-sign-on"></a><span data-ttu-id="78d1e-215">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="78d1e-215">Testing single sign-on</span></span>

<span data-ttu-id="78d1e-216">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="78d1e-216">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="78d1e-217">Ha a hozzáférési Panel hello hello Inkling csempe gombra kattint, automatikusan bejelentkezett tooyour Inkling alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="78d1e-217">When you click hello Inkling tile in hello Access Panel, you should get automatically signed-on tooyour Inkling application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="78d1e-218">További források</span><span class="sxs-lookup"><span data-stu-id="78d1e-218">Additional resources</span></span>

* [<span data-ttu-id="78d1e-219">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="78d1e-219">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="78d1e-220">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="78d1e-220">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-inkling-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-inkling-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-inkling-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-inkling-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-inkling-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-inkling-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-inkling-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-inkling-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-inkling-tutorial/tutorial_general_203.png