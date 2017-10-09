---
title: "Oktatóanyag: Azure Active Directoryval integrált MaxxPoint |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és MaxxPoint között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 15ba026e-96fc-4ae8-b135-0169da810e99
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/13/2017
ms.author: jeedes
ms.openlocfilehash: 03b13908add8d8c62f1d1480ed2288658fce14d4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-maxxpoint"></a><span data-ttu-id="11f48-103">Oktatóanyag: Azure Active Directoryval integrált MaxxPoint</span><span class="sxs-lookup"><span data-stu-id="11f48-103">Tutorial: Azure Active Directory integration with MaxxPoint</span></span>

<span data-ttu-id="11f48-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate MaxxPoint az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="11f48-104">In this tutorial, you learn how toointegrate MaxxPoint with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="11f48-105">MaxxPoint integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="11f48-105">Integrating MaxxPoint with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="11f48-106">Megadhatja a hozzáférés tooMaxxPoint rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="11f48-106">You can control in Azure AD who has access tooMaxxPoint</span></span>
- <span data-ttu-id="11f48-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooMaxxPoint (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="11f48-107">You can enable your users tooautomatically get signed-on tooMaxxPoint (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="11f48-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="11f48-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="11f48-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="11f48-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="11f48-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="11f48-110">Prerequisites</span></span>

<span data-ttu-id="11f48-111">az Azure AD integrálása MaxxPoint tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="11f48-111">tooconfigure Azure AD integration with MaxxPoint, you need hello following items:</span></span>

- <span data-ttu-id="11f48-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="11f48-112">An Azure AD subscription</span></span>
- <span data-ttu-id="11f48-113">Egy MaxxPoint egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="11f48-113">A MaxxPoint single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="11f48-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="11f48-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="11f48-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="11f48-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="11f48-116">Ne használja az éles környezetben, ha ez nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="11f48-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="11f48-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="11f48-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="11f48-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="11f48-118">Scenario description</span></span>
<span data-ttu-id="11f48-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="11f48-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="11f48-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="11f48-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="11f48-121">Hello gyűjteményből MaxxPoint hozzáadása</span><span class="sxs-lookup"><span data-stu-id="11f48-121">Adding MaxxPoint from hello gallery</span></span>
2. <span data-ttu-id="11f48-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="11f48-122">Configuring and testing Azure AD single sign-on</span></span>


## <a name="adding-maxxpoint-from-hello-gallery"></a><span data-ttu-id="11f48-123">Hello gyűjteményből MaxxPoint hozzáadása</span><span class="sxs-lookup"><span data-stu-id="11f48-123">Adding MaxxPoint from hello gallery</span></span>
<span data-ttu-id="11f48-124">tooconfigure hello integrációja MaxxPoint az Azure AD-be, meg kell tooadd MaxxPoint hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="11f48-124">tooconfigure hello integration of MaxxPoint into Azure AD, you need tooadd MaxxPoint from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="11f48-125">**tooadd MaxxPoint hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="11f48-125">**tooadd MaxxPoint from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="11f48-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="11f48-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="11f48-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="11f48-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="11f48-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="11f48-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="11f48-131">Kattintson a **új alkalmazás** gombra a párbeszédpanel tooadd új alkalmazás hello tetején.</span><span class="sxs-lookup"><span data-stu-id="11f48-131">Click **New application** button on hello top of dialog tooadd new application.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="11f48-133">Hello keresési mezőbe, írja be a **MaxxPoint**.</span><span class="sxs-lookup"><span data-stu-id="11f48-133">In hello search box, type **MaxxPoint**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-maxxpoint-tutorial/tutorial_maxxpoint_001.png)

5. <span data-ttu-id="11f48-135">A hello eredmények panelen válassza ki a **MaxxPoint**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="11f48-135">In hello results panel, select **MaxxPoint**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-maxxpoint-tutorial/tutorial_maxxpoint_0001.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="11f48-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="11f48-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="11f48-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján MaxxPoint.</span><span class="sxs-lookup"><span data-stu-id="11f48-138">In this section, you configure and test Azure AD single sign-on with MaxxPoint based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="11f48-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó MaxxPoint tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="11f48-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in MaxxPoint is tooa user in Azure AD.</span></span> <span data-ttu-id="11f48-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello MaxxPoint közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="11f48-140">In other words, a link relationship between an Azure AD user and hello related user in MaxxPoint needs toobe established.</span></span>

<span data-ttu-id="11f48-141">Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** a MaxxPoint.</span><span class="sxs-lookup"><span data-stu-id="11f48-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in MaxxPoint.</span></span>

<span data-ttu-id="11f48-142">tooconfigure és az Azure AD az egyszeri bejelentkezés MaxxPoint-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="11f48-142">tooconfigure and test Azure AD single sign-on with MaxxPoint, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="11f48-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="11f48-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="11f48-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="11f48-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="11f48-145">**[MaxxPoint tesztfelhasználó létrehozása](#creating-a-maxxpoint-test-user)**  -toohave Britta Simon MaxxPoint, amely az Azure AD csatolt toohello ábrázolása rá, hogy az egy megfelelője.</span><span class="sxs-lookup"><span data-stu-id="11f48-145">**[Creating a MaxxPoint test user](#creating-a-maxxpoint-test-user)** - toohave a counterpart of Britta Simon in MaxxPoint that is linked toohello Azure AD representation of her.</span></span>
4. <span data-ttu-id="11f48-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="11f48-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="11f48-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="11f48-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="11f48-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="11f48-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="11f48-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az MaxxPoint alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="11f48-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your MaxxPoint application.</span></span>

<span data-ttu-id="11f48-150">**az Azure AD tooconfigure egyszeri bejelentkezést a MaxxPoint, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="11f48-150">**tooconfigure Azure AD single sign-on with MaxxPoint, perform hello following steps:**</span></span>

1. <span data-ttu-id="11f48-151">Az Azure portál, a hello hello **MaxxPoint** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="11f48-151">In hello Azure portal, on hello **MaxxPoint** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="11f48-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="11f48-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-maxxpoint-tutorial/tutorial_general_300.png)

3. <span data-ttu-id="11f48-155">A hello **MaxxPoint tartomány és az URL-címek** szakaszban, ha tooconfigure hello alkalmazás **IDP kezdeményezett mód**, nem tooperform bármely lépést is végre kell.</span><span class="sxs-lookup"><span data-stu-id="11f48-155">On hello **MaxxPoint Domain and URLs** section, If you wish tooconfigure hello application in **IDP initiated mode**, no need tooperform any steps.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-maxxpoint-tutorial/tutorial_maxxpoint_02.png)
    
4. <span data-ttu-id="11f48-157">A hello **MaxxPoint tartomány és az URL-címek** szakaszban, ha tooconfigure hello alkalmazás **Szolgáltató kezdeményezett mód**, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="11f48-157">On hello **MaxxPoint Domain and URLs** section, If you wish tooconfigure hello application in **SP initiated mode**, perform hello following steps:</span></span>
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-maxxpoint-tutorial/tutorial_maxxpoint_03.png)

    <span data-ttu-id="11f48-159">a.</span><span class="sxs-lookup"><span data-stu-id="11f48-159">a.</span></span> <span data-ttu-id="11f48-160">Kattintson a **megjelenítése speciális URL-beállításainak** beállítás</span><span class="sxs-lookup"><span data-stu-id="11f48-160">Click **Show advanced URL settings** option</span></span>

    <span data-ttu-id="11f48-161">b.</span><span class="sxs-lookup"><span data-stu-id="11f48-161">b.</span></span> <span data-ttu-id="11f48-162">A hello **URL-cím bejelentkezési** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://maxxpoint.westipc.com/default/sso/login/entity/<customer-id>-azure`</span><span class="sxs-lookup"><span data-stu-id="11f48-162">In hello **Sign On URL** textbox, type a URL using hello following pattern: `https://maxxpoint.westipc.com/default/sso/login/entity/<customer-id>-azure`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="11f48-163">Ne feledje, hogy ez a nem hello valódi értékek.</span><span class="sxs-lookup"><span data-stu-id="11f48-163">Please note that this is not hello real value.</span></span> <span data-ttu-id="11f48-164">Ezt az értéket hello tényleges bejelentkezési URL-cím tooupdate rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="11f48-164">You have tooupdate this value with hello actual Sign On URL.</span></span> <span data-ttu-id="11f48-165">MaxxPoint team hívható meg **888-728-0950** tooget ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="11f48-165">Call MaxxPoint team on **888-728-0950** tooget this value.</span></span>

5. <span data-ttu-id="11f48-166">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="11f48-166">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-maxxpoint-tutorial/tutorial_maxxpoint_06.png) 

6. <span data-ttu-id="11f48-168">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="11f48-168">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-maxxpoint-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="11f48-170">az alkalmazáshoz konfigurált SSO tooget, MaxxPoint támogatási csoportjához hívjuk a **888-728-0950** és azok lesz segítséget nyújt további a hogyan őket hello tooprovide letöltött **metaadatainak XML-kódja** fájl.</span><span class="sxs-lookup"><span data-stu-id="11f48-170">tooget SSO configured for your application, call MaxxPoint support team on **888-728-0950** and they'll assist you further on how tooprovide them hello downloaded **Metadata XML** file.</span></span> 

> [!TIP]
> <span data-ttu-id="11f48-171">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="11f48-171">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="11f48-172">Ez az alkalmazás hozzáadása a hello után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="11f48-172">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="11f48-173">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="11f48-173">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="11f48-174">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="11f48-174">Creating an Azure AD test user</span></span>
<span data-ttu-id="11f48-175">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="11f48-175">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="11f48-177">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="11f48-177">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="11f48-178">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="11f48-178">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-maxxpoint-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="11f48-180">Nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó** toodisplay hello azoknak a felhasználóknak.</span><span class="sxs-lookup"><span data-stu-id="11f48-180">Go too**Users and groups** and click **All users** toodisplay hello list of users.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-maxxpoint-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="11f48-182">Hello párbeszédpanel hello tetején kattintson **Hozzáadás** tooopen hello **felhasználói** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="11f48-182">At hello top of hello dialog click **Add** tooopen hello **User** dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-maxxpoint-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="11f48-184">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="11f48-184">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-maxxpoint-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="11f48-186">a.</span><span class="sxs-lookup"><span data-stu-id="11f48-186">a.</span></span> <span data-ttu-id="11f48-187">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="11f48-187">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="11f48-188">b.</span><span class="sxs-lookup"><span data-stu-id="11f48-188">b.</span></span> <span data-ttu-id="11f48-189">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="11f48-189">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="11f48-190">c.</span><span class="sxs-lookup"><span data-stu-id="11f48-190">c.</span></span> <span data-ttu-id="11f48-191">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="11f48-191">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="11f48-192">d.</span><span class="sxs-lookup"><span data-stu-id="11f48-192">d.</span></span> <span data-ttu-id="11f48-193">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="11f48-193">Click **Create**.</span></span> 

### <a name="creating-a-maxxpoint-test-user"></a><span data-ttu-id="11f48-194">MaxxPoint tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="11f48-194">Creating a MaxxPoint test user</span></span>

<span data-ttu-id="11f48-195">Ebben a szakaszban egy MaxxPoint Britta Simon nevű felhasználót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="11f48-195">In this section, you create a user called Britta Simon in MaxxPoint.</span></span> <span data-ttu-id="11f48-196">Hívja a MaxxPoint támogatási csoport **888-728-0950** tooadd hello felhasználók hello MaxxPoint alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="11f48-196">Please call MaxxPoint support team on **888-728-0950** tooadd hello users in hello MaxxPoint application.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="11f48-197">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="11f48-197">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="11f48-198">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés saját hozzáférés tooMaxxPoint megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="11f48-198">In this section, you enable Britta Simon toouse Azure single sign-on by granting her access tooMaxxPoint.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="11f48-200">**tooassign Britta Simon tooMaxxPoint, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="11f48-200">**tooassign Britta Simon tooMaxxPoint, perform hello following steps:**</span></span>

1. <span data-ttu-id="11f48-201">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="11f48-201">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="11f48-203">Hello alkalmazások listában válassza ki a **MaxxPoint**.</span><span class="sxs-lookup"><span data-stu-id="11f48-203">In hello applications list, select **MaxxPoint**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-maxxpoint-tutorial/tutorial_maxxpoint_50.png) 

3. <span data-ttu-id="11f48-205">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="11f48-205">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="11f48-207">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="11f48-207">Click **Add** button.</span></span> <span data-ttu-id="11f48-208">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="11f48-208">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="11f48-210">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="11f48-210">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="11f48-211">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="11f48-211">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="11f48-212">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="11f48-212">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="11f48-213">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="11f48-213">Testing single sign-on</span></span>

<span data-ttu-id="11f48-214">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="11f48-214">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="11f48-215">Ha a hozzáférési Panel hello hello MaxxPoint csempe gombra kattint, automatikusan bejelentkezett tooyour MaxxPoint alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="11f48-215">When you click hello MaxxPoint tile in hello Access Panel, you should get automatically signed-on tooyour MaxxPoint application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="11f48-216">További források</span><span class="sxs-lookup"><span data-stu-id="11f48-216">Additional resources</span></span>

* [<span data-ttu-id="11f48-217">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="11f48-217">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="11f48-218">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="11f48-218">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-maxxpoint-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-maxxpoint-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-maxxpoint-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-maxxpoint-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-maxxpoint-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-maxxpoint-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-maxxpoint-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-maxxpoint-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-maxxpoint-tutorial/tutorial_general_203.png