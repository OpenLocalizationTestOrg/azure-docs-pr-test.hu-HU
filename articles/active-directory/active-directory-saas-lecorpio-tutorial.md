---
title: "Oktatóanyag: Azure Active Directoryval integrált Lecorpio |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Lecorpio között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 28acce3e-22a0-4a37-8b66-6e518d777350
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/02/2017
ms.author: jeedes
ms.openlocfilehash: 963eb36678c589f942f63c7ab555161255324717
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-lecorpio"></a><span data-ttu-id="9abf0-103">Oktatóanyag: Azure Active Directoryval integrált Lecorpio</span><span class="sxs-lookup"><span data-stu-id="9abf0-103">Tutorial: Azure Active Directory integration with Lecorpio</span></span>

<span data-ttu-id="9abf0-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Lecorpio az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="9abf0-104">In this tutorial, you learn how toointegrate Lecorpio with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="9abf0-105">Lecorpio integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="9abf0-105">Integrating Lecorpio with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="9abf0-106">Megadhatja a hozzáférés tooLecorpio rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="9abf0-106">You can control in Azure AD who has access tooLecorpio</span></span>
- <span data-ttu-id="9abf0-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooLecorpio (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="9abf0-107">You can enable your users tooautomatically get signed-on tooLecorpio (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="9abf0-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="9abf0-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="9abf0-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="9abf0-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9abf0-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="9abf0-110">Prerequisites</span></span>

<span data-ttu-id="9abf0-111">az Azure AD integrálása Lecorpio tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="9abf0-111">tooconfigure Azure AD integration with Lecorpio, you need hello following items:</span></span>

- <span data-ttu-id="9abf0-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="9abf0-112">An Azure AD subscription</span></span>
- <span data-ttu-id="9abf0-113">Egy Lecorpio egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="9abf0-113">A Lecorpio single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="9abf0-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="9abf0-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="9abf0-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="9abf0-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="9abf0-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="9abf0-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="9abf0-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="9abf0-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="9abf0-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="9abf0-118">Scenario description</span></span>
<span data-ttu-id="9abf0-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="9abf0-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="9abf0-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="9abf0-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="9abf0-121">Hello gyűjteményből Lecorpio hozzáadása</span><span class="sxs-lookup"><span data-stu-id="9abf0-121">Adding Lecorpio from hello gallery</span></span>
2. <span data-ttu-id="9abf0-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="9abf0-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-lecorpio-from-hello-gallery"></a><span data-ttu-id="9abf0-123">Hello gyűjteményből Lecorpio hozzáadása</span><span class="sxs-lookup"><span data-stu-id="9abf0-123">Adding Lecorpio from hello gallery</span></span>
<span data-ttu-id="9abf0-124">tooconfigure hello integrációja Lecorpio az Azure AD-be, meg kell tooadd Lecorpio hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="9abf0-124">tooconfigure hello integration of Lecorpio into Azure AD, you need tooadd Lecorpio from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="9abf0-125">**tooadd Lecorpio hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="9abf0-125">**tooadd Lecorpio from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="9abf0-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="9abf0-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="9abf0-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="9abf0-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="9abf0-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="9abf0-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="9abf0-131">Kattintson a **új alkalmazás** hello párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="9abf0-131">Click **New application** button on hello top of hello dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="9abf0-133">Hello keresési mezőbe, írja be a **Lecorpio**.</span><span class="sxs-lookup"><span data-stu-id="9abf0-133">In hello search box, type **Lecorpio**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-lecorpio-tutorial/tutorial_lecorpio_search.png)

5. <span data-ttu-id="9abf0-135">A hello eredmények panelen válassza ki a **Lecorpio**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="9abf0-135">In hello results panel, select **Lecorpio**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-lecorpio-tutorial/tutorial_lecorpio_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="9abf0-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="9abf0-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="9abf0-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján Lecorpio</span><span class="sxs-lookup"><span data-stu-id="9abf0-138">In this section, you configure and test Azure AD single sign-on with Lecorpio based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="9abf0-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Lecorpio tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="9abf0-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Lecorpio is tooa user in Azure AD.</span></span> <span data-ttu-id="9abf0-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Lecorpio közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="9abf0-140">In other words, a link relationship between an Azure AD user and hello related user in Lecorpio needs toobe established.</span></span>

<span data-ttu-id="9abf0-141">Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** a Lecorpio.</span><span class="sxs-lookup"><span data-stu-id="9abf0-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Lecorpio.</span></span>

<span data-ttu-id="9abf0-142">tooconfigure és az Azure AD az egyszeri bejelentkezés Lecorpio-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="9abf0-142">tooconfigure and test Azure AD single sign-on with Lecorpio, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="9abf0-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="9abf0-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="9abf0-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="9abf0-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="9abf0-145">**[Lecorpio tesztfelhasználó létrehozása](#creating-a-lecorpio-test-user)**  -toohave egy megfelelője a Britta Simon a Lecorpio, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="9abf0-145">**[Creating a Lecorpio test user](#creating-a-lecorpio-test-user)** - toohave a counterpart of Britta Simon in Lecorpio that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="9abf0-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="9abf0-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="9abf0-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="9abf0-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="9abf0-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="9abf0-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="9abf0-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Lecorpio alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="9abf0-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Lecorpio application.</span></span>

<span data-ttu-id="9abf0-150">**az Azure AD tooconfigure egyszeri bejelentkezést a Lecorpio, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="9abf0-150">**tooconfigure Azure AD single sign-on with Lecorpio, perform hello following steps:**</span></span>

1. <span data-ttu-id="9abf0-151">Az Azure portál, a hello hello **Lecorpio** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="9abf0-151">In hello Azure portal, on hello **Lecorpio** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="9abf0-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="9abf0-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-lecorpio-tutorial/tutorial_lecorpio_samlbase.png)

3. <span data-ttu-id="9abf0-155">A hello **Lecorpio tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="9abf0-155">On hello **Lecorpio Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-lecorpio-tutorial/tutorial_lecorpio_url.png)

    <span data-ttu-id="9abf0-157">a.</span><span class="sxs-lookup"><span data-stu-id="9abf0-157">a.</span></span> <span data-ttu-id="9abf0-158">A hello **bejelentkezési URL-cím** szövegmezőhöz típusú hello érték a következő mintát hello használata:`https://<instance name>.lecorpio.com/<customer name>`</span><span class="sxs-lookup"><span data-stu-id="9abf0-158">In hello **Sign-on URL** textbox, type hello value using hello following pattern: `https://<instance name>.lecorpio.com/<customer name>`</span></span>

    <span data-ttu-id="9abf0-159">b.</span><span class="sxs-lookup"><span data-stu-id="9abf0-159">b.</span></span> <span data-ttu-id="9abf0-160">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<instance name>.lecorpio.com/<customer name>`</span><span class="sxs-lookup"><span data-stu-id="9abf0-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<instance name>.lecorpio.com/<customer name>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="9abf0-161">Ezek az értékek nincsenek hello valós.</span><span class="sxs-lookup"><span data-stu-id="9abf0-161">These values are not hello real.</span></span> <span data-ttu-id="9abf0-162">Frissítheti ezeket az értékeket a hello tényleges bejelentkezési URL-cím és azonosítója.</span><span class="sxs-lookup"><span data-stu-id="9abf0-162">Update these values with hello actual Sign-on URL and Identifier.</span></span> <span data-ttu-id="9abf0-163">Itt javasoljuk, hogy Ön toouse hello egyedi karakterlánc értéke a hello azonosítója.</span><span class="sxs-lookup"><span data-stu-id="9abf0-163">Here we suggest you toouse hello unique value of string in hello Identifier.</span></span> <span data-ttu-id="9abf0-164">Ügyfél [Lecorpio ügyfél-támogatási csoport](mailto:info@lecorpio.com) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="9abf0-164">Contact [Lecorpio Client support team](mailto:info@lecorpio.com) tooget these values.</span></span> 
 
4. <span data-ttu-id="9abf0-165">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="9abf0-165">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-lecorpio-tutorial/tutorial_lecorpio_certificate.png) 

5. <span data-ttu-id="9abf0-167">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="9abf0-167">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-lecorpio-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="9abf0-169">tooconfigure egyszeri bejelentkezést a **Lecorpio** oldalon kell letöltött toosend hello **metaadatainak XML-kódja** túl[Lecorpio támogatási csoport](mailto:info@lecorpio.com).</span><span class="sxs-lookup"><span data-stu-id="9abf0-169">tooconfigure single sign-on on **Lecorpio** side, you need toosend hello downloaded **Metadata XML** too[Lecorpio support team](mailto:info@lecorpio.com).</span></span>

> [!TIP]
> <span data-ttu-id="9abf0-170">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="9abf0-170">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="9abf0-171">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="9abf0-171">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="9abf0-172">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="9abf0-172">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="9abf0-173">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="9abf0-173">Creating an Azure AD test user</span></span>
<span data-ttu-id="9abf0-174">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="9abf0-174">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="9abf0-176">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="9abf0-176">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="9abf0-177">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="9abf0-177">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-lecorpio-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="9abf0-179">Nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó** toodisplay hello azoknak a felhasználóknak.</span><span class="sxs-lookup"><span data-stu-id="9abf0-179">Go too**Users and groups** and click **All users** toodisplay hello list of users.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-lecorpio-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="9abf0-181">Hello hello párbeszédpanel, kattintson tetején **Hozzáadás** tooopen hello **felhasználói** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="9abf0-181">At hello top of hello dialog, click **Add** tooopen hello **User** dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-lecorpio-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="9abf0-183">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="9abf0-183">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-lecorpio-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="9abf0-185">a.</span><span class="sxs-lookup"><span data-stu-id="9abf0-185">a.</span></span> <span data-ttu-id="9abf0-186">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="9abf0-186">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="9abf0-187">b.</span><span class="sxs-lookup"><span data-stu-id="9abf0-187">b.</span></span> <span data-ttu-id="9abf0-188">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="9abf0-188">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="9abf0-189">c.</span><span class="sxs-lookup"><span data-stu-id="9abf0-189">c.</span></span> <span data-ttu-id="9abf0-190">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="9abf0-190">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="9abf0-191">d.</span><span class="sxs-lookup"><span data-stu-id="9abf0-191">d.</span></span> <span data-ttu-id="9abf0-192">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="9abf0-192">Click **Create**.</span></span>
 
### <a name="creating-a-lecorpio-test-user"></a><span data-ttu-id="9abf0-193">Lecorpio tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="9abf0-193">Creating a Lecorpio test user</span></span>

<span data-ttu-id="9abf0-194">Ebben a szakaszban egy Lecorpio Britta Simon nevű felhasználót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="9abf0-194">In this section, you create a user called Britta Simon in Lecorpio.</span></span> 

<span data-ttu-id="9abf0-195">Ügyfél [Lecorpio ügyfél-támogatási csoport](mailto:info@lecorpio.com) tooadd hello felhasználók hello Lecorpio alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="9abf0-195">Contact [Lecorpio Client support team](mailto:info@lecorpio.com) tooadd hello users in hello Lecorpio application.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="9abf0-196">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="9abf0-196">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="9abf0-197">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooLecorpio megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="9abf0-197">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooLecorpio.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="9abf0-199">**tooassign Britta Simon tooLecorpio, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="9abf0-199">**tooassign Britta Simon tooLecorpio, perform hello following steps:**</span></span>

1. <span data-ttu-id="9abf0-200">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="9abf0-200">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="9abf0-202">Hello alkalmazások listában válassza ki a **Lecorpio**.</span><span class="sxs-lookup"><span data-stu-id="9abf0-202">In hello applications list, select **Lecorpio**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-lecorpio-tutorial/tutorial_lecorpio_app.png) 

3. <span data-ttu-id="9abf0-204">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="9abf0-204">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="9abf0-206">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="9abf0-206">Click **Add** button.</span></span> <span data-ttu-id="9abf0-207">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="9abf0-207">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="9abf0-209">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="9abf0-209">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="9abf0-210">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="9abf0-210">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="9abf0-211">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="9abf0-211">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="9abf0-212">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="9abf0-212">Testing single sign-on</span></span>

<span data-ttu-id="9abf0-213">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="9abf0-213">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="9abf0-214">Ha a hozzáférési Panel hello hello Lecorpio csempe gombra kattint, automatikusan bejelentkezett tooyour Lecorpio alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="9abf0-214">When you click hello Lecorpio tile in hello Access Panel, you should get automatically signed-on tooyour Lecorpio application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9abf0-215">További források</span><span class="sxs-lookup"><span data-stu-id="9abf0-215">Additional resources</span></span>

* [<span data-ttu-id="9abf0-216">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="9abf0-216">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="9abf0-217">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="9abf0-217">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-lecorpio-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-lecorpio-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-lecorpio-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-lecorpio-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-lecorpio-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-lecorpio-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-lecorpio-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-lecorpio-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-lecorpio-tutorial/tutorial_general_203.png

