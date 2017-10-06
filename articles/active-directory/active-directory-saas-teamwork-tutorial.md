---
title: "Oktatóanyag: Azure Active Directoryval integrált alkották |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és alkották között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 03760032-3d76-4b47-ab84-241f72fbd561
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/15/2017
ms.author: jeedes
ms.openlocfilehash: f3a88a146f2a0a70de5ef58abd46f7f26b4104f3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-teamwork"></a><span data-ttu-id="b0c95-103">Oktatóanyag: Azure Active Directoryval integrált alkották</span><span class="sxs-lookup"><span data-stu-id="b0c95-103">Tutorial: Azure Active Directory integration with Teamwork</span></span>

<span data-ttu-id="b0c95-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate alkották az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="b0c95-104">In this tutorial, you learn how toointegrate Teamwork with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="b0c95-105">Alkották integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="b0c95-105">Integrating Teamwork with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="b0c95-106">Megadhatja a hozzáférés tooTeamwork rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="b0c95-106">You can control in Azure AD who has access tooTeamwork</span></span>
- <span data-ttu-id="b0c95-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooTeamwork (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="b0c95-107">You can enable your users tooautomatically get signed-on tooTeamwork (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="b0c95-108">Kezelheti a fiókokat, egy központi helyen - hello Azure felügyeleti portálon</span><span class="sxs-lookup"><span data-stu-id="b0c95-108">You can manage your accounts in one central location - hello Azure Management portal</span></span>

<span data-ttu-id="b0c95-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="b0c95-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b0c95-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="b0c95-110">Prerequisites</span></span>

<span data-ttu-id="b0c95-111">az Azure AD integrálása alkották tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="b0c95-111">tooconfigure Azure AD integration with Teamwork, you need hello following items:</span></span>

- <span data-ttu-id="b0c95-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="b0c95-112">An Azure AD subscription</span></span>
- <span data-ttu-id="b0c95-113">Egy alkották egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="b0c95-113">A Teamwork single-sign on enabled subscription</span></span>


> [!NOTE]
> <span data-ttu-id="b0c95-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="b0c95-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>


<span data-ttu-id="b0c95-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="b0c95-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="b0c95-116">Ne használja az éles környezetben, ha ez nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="b0c95-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="b0c95-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="b0c95-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>


## <a name="scenario-description"></a><span data-ttu-id="b0c95-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="b0c95-118">Scenario description</span></span>
<span data-ttu-id="b0c95-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="b0c95-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="b0c95-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="b0c95-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="b0c95-121">Hello gyűjteményből alkották hozzáadása</span><span class="sxs-lookup"><span data-stu-id="b0c95-121">Adding Teamwork from hello gallery</span></span>
2. <span data-ttu-id="b0c95-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="b0c95-122">Configuring and testing Azure AD single sign-on</span></span>


## <a name="adding-teamwork-from-hello-gallery"></a><span data-ttu-id="b0c95-123">Hello gyűjteményből alkották hozzáadása</span><span class="sxs-lookup"><span data-stu-id="b0c95-123">Adding Teamwork from hello gallery</span></span>
<span data-ttu-id="b0c95-124">tooconfigure hello integrációs alkották, az Azure AD-be, meg kell tooadd alkották hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="b0c95-124">tooconfigure hello integration of Teamwork into Azure AD, you need tooadd Teamwork from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="b0c95-125">**tooadd alkották hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="b0c95-125">**tooadd Teamwork from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="b0c95-126">A hello  **[Azure felügyeleti portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="b0c95-126">In hello **[Azure Management Portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="b0c95-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="b0c95-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="b0c95-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="b0c95-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="b0c95-131">Kattintson a **Hozzáadás** hello párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="b0c95-131">Click **Add** button on hello top of hello dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="b0c95-133">Hello keresési mezőbe, írja be a **alkották**.</span><span class="sxs-lookup"><span data-stu-id="b0c95-133">In hello search box, type **Teamwork**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-teamwork-tutorial/tutorial_teamwork_001.png)

5. <span data-ttu-id="b0c95-135">A hello eredmények panelen válassza ki a **alkották**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="b0c95-135">In hello results panel, select **Teamwork**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-teamwork-tutorial/tutorial_teamwork_0001.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="b0c95-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="b0c95-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="b0c95-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján alkották.</span><span class="sxs-lookup"><span data-stu-id="b0c95-138">In this section, you configure and test Azure AD single sign-on with Teamwork based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="b0c95-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó alkották tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="b0c95-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Teamwork is tooa user in Azure AD.</span></span> <span data-ttu-id="b0c95-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello alkották közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="b0c95-140">In other words, a link relationship between an Azure AD user and hello related user in Teamwork needs toobe established.</span></span>

<span data-ttu-id="b0c95-141">Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** alkották a.</span><span class="sxs-lookup"><span data-stu-id="b0c95-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Teamwork.</span></span>

<span data-ttu-id="b0c95-142">tooconfigure és az Azure AD az egyszeri bejelentkezés alkották-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="b0c95-142">tooconfigure and test Azure AD single sign-on with Teamwork, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="b0c95-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="b0c95-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="b0c95-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b0c95-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="b0c95-145">**[Alkották tesztfelhasználó létrehozása](#creating-a-teamwork-test-user)**  -toohave Britta Simon alkották, amely az Azure AD csatolt toohello ábrázolása rá, hogy az egy megfelelője.</span><span class="sxs-lookup"><span data-stu-id="b0c95-145">**[Creating a Teamwork test user](#creating-a-teamwork-test-user)** - toohave a counterpart of Britta Simon in Teamwork that is linked toohello Azure AD representation of her.</span></span>
4. <span data-ttu-id="b0c95-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="b0c95-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="b0c95-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="b0c95-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="b0c95-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b0c95-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="b0c95-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel hello Azure felügyeleti portálon, és konfigurálása egyszeri bejelentkezéshez az alkották alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="b0c95-149">In this section, you enable Azure AD single sign-on in hello Azure Management portal and configure single sign-on in your Teamwork application.</span></span>

<span data-ttu-id="b0c95-150">**az Azure AD tooconfigure egyszeri bejelentkezést a alkották, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="b0c95-150">**tooconfigure Azure AD single sign-on with Teamwork, perform hello following steps:**</span></span>

1. <span data-ttu-id="b0c95-151">Hello Azure felügyeleti portálon, a hello **alkották** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="b0c95-151">In hello Azure Management portal, on hello **Teamwork** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="b0c95-153">A hello **egyszeri bejelentkezés** párbeszédpanel, mint **mód** kiválasztása **SAML-alapú bejelentkezés** tooenable az egyszeri bejelentkezés.</span><span class="sxs-lookup"><span data-stu-id="b0c95-153">On hello **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** tooenable single sign on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-teamwork-tutorial/tutorial_teamwork_01.png)

3. <span data-ttu-id="b0c95-155">A hello **alkották tartomány és az URL-címek** című hello **URL-cím bejelentkezési** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<company name>.teamwork.com`</span><span class="sxs-lookup"><span data-stu-id="b0c95-155">On hello **Teamwork Domain and URLs** section, in hello **Sign On URL** textbox, type a URL using hello following pattern: `https://<company name>.teamwork.com`</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-teamwork-tutorial/tutorial_teamwork_02.png)

    > [!NOTE] 
    > <span data-ttu-id="b0c95-157">Ne feledje, hogy ez a nem hello valódi értékek.</span><span class="sxs-lookup"><span data-stu-id="b0c95-157">Please note that this is not hello real value.</span></span> <span data-ttu-id="b0c95-158">Ezt az értéket hello tényleges bejelentkezési URL-cím tooupdate rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="b0c95-158">You have tooupdate this value with hello actual Sign On URL.</span></span> <span data-ttu-id="b0c95-159">Ügyfél [alkották támogatási csoport](mailto:support@teamwork.com) tooget ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="b0c95-159">Contact [Teamwork support team](mailto:support@teamwork.com) tooget this value.</span></span> 

4. <span data-ttu-id="b0c95-160">A hello **SAML-aláíró tanúsítványa** kattintson **hozzon létre új tanúsítvány**.</span><span class="sxs-lookup"><span data-stu-id="b0c95-160">On hello **SAML Signing Certificate** section, click **Create new certificate**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-teamwork-tutorial/tutorial_teamwork_03.png)   

5. <span data-ttu-id="b0c95-162">A hello **új tanúsítvány létrehozása** párbeszédpanelen hello naptár ikonra, és válassza ki az **lejárati dátum**.</span><span class="sxs-lookup"><span data-stu-id="b0c95-162">On hello **Create New Certificate** dialog, click hello calendar icon and select an **expiry date**.</span></span> <span data-ttu-id="b0c95-163">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="b0c95-163">Then click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-teamwork-tutorial/tutorial_general_300.png)

6. <span data-ttu-id="b0c95-165">A hello **SAML-aláíró tanúsítványa** szakaszban jelölje be **új tanúsítvány aktiválásához** kattintson **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="b0c95-165">On hello **SAML Signing Certificate** section, select **Make new certificate active** and click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-teamwork-tutorial/tutorial_teamwork_04.png)

7. <span data-ttu-id="b0c95-167">A hello előugró ablak **helyettesítő tanúsítvány** ablak, kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="b0c95-167">On hello pop-up **Rollover certificate** window, click **OK**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-teamwork-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="b0c95-169">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="b0c95-169">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-teamwork-tutorial/tutorial_teamwork_05.png) 

9. <span data-ttu-id="b0c95-171">az alkalmazáshoz konfigurált SSO tooget, forduljon a [alkották támogatási csoport](mailto:support@teamwork.com) és adja meg a letöltött hello **metaadatok**.</span><span class="sxs-lookup"><span data-stu-id="b0c95-171">tooget SSO configured for your application, contact [Teamwork support team](mailto:support@teamwork.com) and provide them with hello downloaded **metadata**.</span></span>
  

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="b0c95-172">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="b0c95-172">Creating an Azure AD test user</span></span>
<span data-ttu-id="b0c95-173">hello ebben a szakaszban célja toocreate tesztfelhasználó Britta Simon nevű hello Azure felügyeleti portálon.</span><span class="sxs-lookup"><span data-stu-id="b0c95-173">hello objective of this section is toocreate a test user in hello Azure Management portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="b0c95-175">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="b0c95-175">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="b0c95-176">A hello **Azure Management portal**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="b0c95-176">In hello **Azure Management portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-teamwork-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="b0c95-178">Nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó** toodisplay hello azoknak a felhasználóknak.</span><span class="sxs-lookup"><span data-stu-id="b0c95-178">Go too**Users and groups** and click **All users** toodisplay hello list of users.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-teamwork-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="b0c95-180">Hello párbeszédpanel hello tetején kattintson **Hozzáadás** tooopen hello **felhasználói** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="b0c95-180">At hello top of hello dialog click **Add** tooopen hello **User** dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-teamwork-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="b0c95-182">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="b0c95-182">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-teamwork-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="b0c95-184">a.</span><span class="sxs-lookup"><span data-stu-id="b0c95-184">a.</span></span> <span data-ttu-id="b0c95-185">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="b0c95-185">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="b0c95-186">b.</span><span class="sxs-lookup"><span data-stu-id="b0c95-186">b.</span></span> <span data-ttu-id="b0c95-187">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="b0c95-187">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="b0c95-188">c.</span><span class="sxs-lookup"><span data-stu-id="b0c95-188">c.</span></span> <span data-ttu-id="b0c95-189">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="b0c95-189">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="b0c95-190">d.</span><span class="sxs-lookup"><span data-stu-id="b0c95-190">d.</span></span> <span data-ttu-id="b0c95-191">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="b0c95-191">Click **Create**.</span></span> 



### <a name="creating-a-teamwork-test-user"></a><span data-ttu-id="b0c95-192">Alkották tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="b0c95-192">Creating a Teamwork test user</span></span>

<span data-ttu-id="b0c95-193">Ebben a szakaszban egy alkották Britta Simon nevű felhasználót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="b0c95-193">In this section, you create a user called Britta Simon in Teamwork.</span></span> <span data-ttu-id="b0c95-194">Adjon együttműködve [alkották támogatási csoport](mailto:support@teamwork.com) tooadd hello felhasználók hello alkották platform.</span><span class="sxs-lookup"><span data-stu-id="b0c95-194">Please work with [Teamwork support team](mailto:support@teamwork.com) tooadd hello users in hello Teamwork platform.</span></span>


### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="b0c95-195">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="b0c95-195">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="b0c95-196">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés saját hozzáférés tooTeamwork megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="b0c95-196">In this section, you enable Britta Simon toouse Azure single sign-on by granting her access tooTeamwork.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="b0c95-198">**tooassign Britta Simon tooTeamwork, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="b0c95-198">**tooassign Britta Simon tooTeamwork, perform hello following steps:**</span></span>

1. <span data-ttu-id="b0c95-199">Hello Azure felügyeleti portálján, nyissa meg a hello alkalmazások megtekintése, majd toohello könyvtár nézetben keresse meg, és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="b0c95-199">In hello Azure Management portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="b0c95-201">Hello alkalmazások listában válassza ki a **alkották**.</span><span class="sxs-lookup"><span data-stu-id="b0c95-201">In hello applications list, select **Teamwork**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-teamwork-tutorial/tutorial_teamwork_50.png) 

3. <span data-ttu-id="b0c95-203">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="b0c95-203">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="b0c95-205">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="b0c95-205">Click **Add** button.</span></span> <span data-ttu-id="b0c95-206">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="b0c95-206">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="b0c95-208">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="b0c95-208">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="b0c95-209">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="b0c95-209">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="b0c95-210">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="b0c95-210">Click **Assign** button on **Add Assignment** dialog.</span></span>
    


### <a name="testing-single-sign-on"></a><span data-ttu-id="b0c95-211">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="b0c95-211">Testing single sign-on</span></span>

<span data-ttu-id="b0c95-212">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="b0c95-212">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="b0c95-213">Ha a hozzáférési Panel hello hello alkották csempe gombra kattint, automatikusan bejelentkezett tooyour alkották alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="b0c95-213">When you click hello Teamwork tile in hello Access Panel, you should get automatically signed-on tooyour Teamwork application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="b0c95-214">További források</span><span class="sxs-lookup"><span data-stu-id="b0c95-214">Additional resources</span></span>

* [<span data-ttu-id="b0c95-215">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="b0c95-215">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="b0c95-216">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="b0c95-216">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-teamwork-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-teamwork-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-teamwork-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-teamwork-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-teamwork-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-teamwork-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-teamwork-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-teamwork-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-teamwork-tutorial/tutorial_general_203.png