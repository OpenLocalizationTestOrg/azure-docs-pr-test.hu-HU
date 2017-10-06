---
title: "Oktatóanyag: Azure Active Directoryval integrált Fuze |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Fuze között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 9780b4bf-1fd1-48c1-9ceb-f750225ae08a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: jeedes
ms.openlocfilehash: d0ea8c6456824e348301ed8bf1f5e00f4bfa8121
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-fuze"></a><span data-ttu-id="2d2bf-103">Oktatóanyag: Azure Active Directoryval integrált Fuze</span><span class="sxs-lookup"><span data-stu-id="2d2bf-103">Tutorial: Azure Active Directory integration with Fuze</span></span>

<span data-ttu-id="2d2bf-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Fuze az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="2d2bf-104">In this tutorial, you learn how toointegrate Fuze with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="2d2bf-105">Fuze integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="2d2bf-105">Integrating Fuze with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="2d2bf-106">Megadhatja a hozzáférés tooFuze rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="2d2bf-106">You can control in Azure AD who has access tooFuze</span></span>
- <span data-ttu-id="2d2bf-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooFuze (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="2d2bf-107">You can enable your users tooautomatically get signed-on tooFuze (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="2d2bf-108">Kezelheti a fiókokat, egy központi helyen - hello Azure felügyeleti portálon</span><span class="sxs-lookup"><span data-stu-id="2d2bf-108">You can manage your accounts in one central location - hello Azure Management portal</span></span>

<span data-ttu-id="2d2bf-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="2d2bf-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2d2bf-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="2d2bf-110">Prerequisites</span></span>

<span data-ttu-id="2d2bf-111">az Azure AD integrálása Fuze tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="2d2bf-111">tooconfigure Azure AD integration with Fuze, you need hello following items:</span></span>

- <span data-ttu-id="2d2bf-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="2d2bf-112">An Azure AD subscription</span></span>
- <span data-ttu-id="2d2bf-113">Egy Fuze egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="2d2bf-113">A Fuze single-sign on enabled subscription</span></span>


> [!NOTE]
> <span data-ttu-id="2d2bf-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="2d2bf-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>


<span data-ttu-id="2d2bf-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="2d2bf-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="2d2bf-116">Ne használja az éles környezetben, ha ez nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="2d2bf-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="2d2bf-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="2d2bf-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>


## <a name="scenario-description"></a><span data-ttu-id="2d2bf-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="2d2bf-118">Scenario description</span></span>
<span data-ttu-id="2d2bf-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="2d2bf-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="2d2bf-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="2d2bf-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="2d2bf-121">Hello gyűjteményből Fuze hozzáadása</span><span class="sxs-lookup"><span data-stu-id="2d2bf-121">Adding Fuze from hello gallery</span></span>
2. <span data-ttu-id="2d2bf-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="2d2bf-122">Configuring and testing Azure AD single sign-on</span></span>


## <a name="adding-fuze-from-hello-gallery"></a><span data-ttu-id="2d2bf-123">Hello gyűjteményből Fuze hozzáadása</span><span class="sxs-lookup"><span data-stu-id="2d2bf-123">Adding Fuze from hello gallery</span></span>
<span data-ttu-id="2d2bf-124">tooconfigure hello integrációja Fuze az Azure AD-be, meg kell tooadd Fuze hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="2d2bf-124">tooconfigure hello integration of Fuze into Azure AD, you need tooadd Fuze from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="2d2bf-125">**tooadd Fuze hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="2d2bf-125">**tooadd Fuze from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="2d2bf-126">A hello  **[Azure felügyeleti portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="2d2bf-126">In hello **[Azure Management Portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="2d2bf-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="2d2bf-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="2d2bf-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="2d2bf-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="2d2bf-131">Kattintson a **Hozzáadás** hello párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="2d2bf-131">Click **Add** button on hello top of hello dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="2d2bf-133">Hello keresési mezőbe, írja be a **Fuze**.</span><span class="sxs-lookup"><span data-stu-id="2d2bf-133">In hello search box, type **Fuze**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-fuze-tutorial/tutorial_fuze_000.png)

5. <span data-ttu-id="2d2bf-135">A hello eredmények panelen válassza ki a **Fuze**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="2d2bf-135">In hello results panel, select **Fuze**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-fuze-tutorial/tutorial_fuze_0001.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="2d2bf-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="2d2bf-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="2d2bf-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Fuze.</span><span class="sxs-lookup"><span data-stu-id="2d2bf-138">In this section, you configure and test Azure AD single sign-on with Fuze based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="2d2bf-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Fuze tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="2d2bf-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Fuze is tooa user in Azure AD.</span></span> <span data-ttu-id="2d2bf-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Fuze közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="2d2bf-140">In other words, a link relationship between an Azure AD user and hello related user in Fuze needs toobe established.</span></span>

<span data-ttu-id="2d2bf-141">Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** a Fuze.</span><span class="sxs-lookup"><span data-stu-id="2d2bf-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Fuze.</span></span>

<span data-ttu-id="2d2bf-142">tooconfigure és az Azure AD az egyszeri bejelentkezés Fuze-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="2d2bf-142">tooconfigure and test Azure AD single sign-on with Fuze, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="2d2bf-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="2d2bf-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="2d2bf-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="2d2bf-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="2d2bf-145">**[Fuze tesztfelhasználó létrehozása](#creating-a-fuze-test-user)**  -toohave Britta Simon Fuze, amely az Azure AD csatolt toohello ábrázolása rá, hogy az egy megfelelője.</span><span class="sxs-lookup"><span data-stu-id="2d2bf-145">**[Creating a Fuze test user](#creating-a-fuze-test-user)** - toohave a counterpart of Britta Simon in Fuze that is linked toohello Azure AD representation of her.</span></span>
4. <span data-ttu-id="2d2bf-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="2d2bf-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="2d2bf-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="2d2bf-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="2d2bf-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="2d2bf-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="2d2bf-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel hello Azure felügyeleti portálon, és konfigurálása egyszeri bejelentkezéshez az Fuze alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="2d2bf-149">In this section, you enable Azure AD single sign-on in hello Azure Management portal and configure single sign-on in your Fuze application.</span></span>

<span data-ttu-id="2d2bf-150">**az Azure AD tooconfigure egyszeri bejelentkezést a Fuze, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="2d2bf-150">**tooconfigure Azure AD single sign-on with Fuze, perform hello following steps:**</span></span>

1. <span data-ttu-id="2d2bf-151">Hello Azure felügyeleti portálon, a hello **Fuze** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="2d2bf-151">In hello Azure Management portal, on hello **Fuze** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="2d2bf-153">A hello **egyszeri bejelentkezés** párbeszédpanel, mint **mód** kiválasztása **SAML-alapú bejelentkezés** tooenable az egyszeri bejelentkezés.</span><span class="sxs-lookup"><span data-stu-id="2d2bf-153">On hello **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** tooenable single sign on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-fuze-tutorial/tutorial_fuze_01.png)

3. <span data-ttu-id="2d2bf-155">A hello **Fuze tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="2d2bf-155">On hello **Fuze Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-fuze-tutorial/tutorial_fuze_020.png)
    
    <span data-ttu-id="2d2bf-157">A hello **bejelentkezési URL-cím** szövegmezőhöz típus hello bejelentkezési URL-címet:`https://www.thinkingphones.com/jetspeed/portal/`</span><span class="sxs-lookup"><span data-stu-id="2d2bf-157">In hello **Sign on URL** textbox, type hello Sign-on URL as: `https://www.thinkingphones.com/jetspeed/portal/`</span></span>

4.  <span data-ttu-id="2d2bf-158">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="2d2bf-158">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-fuze-tutorial/tutorial_general_400.png)

5. <span data-ttu-id="2d2bf-160">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello XML-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="2d2bf-160">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello xml file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-fuze-tutorial/tutorial_fuze_05.png) 

6. <span data-ttu-id="2d2bf-162">tooconfigure egyszeri bejelentkezést a **Fuze** oldalon kell letöltött toosend hello **metaadatainak XML-kódja** túl[Fuze támogatási csoport](https://www.fuze.com/support).</span><span class="sxs-lookup"><span data-stu-id="2d2bf-162">tooconfigure single sign-on on **Fuze** side, you need toosend hello downloaded **Metadata XML** too[Fuze support team](https://www.fuze.com/support).</span></span> <span data-ttu-id="2d2bf-163">Azok fog beállítása ennek rendelés toohave hello SAML SSO kapcsolat mindkét oldalán megfelelően beállítva.</span><span class="sxs-lookup"><span data-stu-id="2d2bf-163">They will set this up in order toohave hello SAML SSO connection set properly on both sides.</span></span>


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="2d2bf-164">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="2d2bf-164">Creating an Azure AD test user</span></span>
<span data-ttu-id="2d2bf-165">hello ebben a szakaszban célja toocreate tesztfelhasználó Britta Simon nevű hello Azure felügyeleti portálon.</span><span class="sxs-lookup"><span data-stu-id="2d2bf-165">hello objective of this section is toocreate a test user in hello Azure Management portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="2d2bf-167">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="2d2bf-167">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="2d2bf-168">A hello **Azure Management portal**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="2d2bf-168">In hello **Azure Management portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-fuze-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="2d2bf-170">Nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó** toodisplay hello azoknak a felhasználóknak.</span><span class="sxs-lookup"><span data-stu-id="2d2bf-170">Go too**Users and groups** and click **All users** toodisplay hello list of users.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-fuze-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="2d2bf-172">Hello párbeszédpanel hello tetején kattintson **Hozzáadás** tooopen hello **felhasználói** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="2d2bf-172">At hello top of hello dialog click **Add** tooopen hello **User** dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-fuze-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="2d2bf-174">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="2d2bf-174">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-fuze-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="2d2bf-176">a.</span><span class="sxs-lookup"><span data-stu-id="2d2bf-176">a.</span></span> <span data-ttu-id="2d2bf-177">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="2d2bf-177">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="2d2bf-178">b.</span><span class="sxs-lookup"><span data-stu-id="2d2bf-178">b.</span></span> <span data-ttu-id="2d2bf-179">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="2d2bf-179">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="2d2bf-180">c.</span><span class="sxs-lookup"><span data-stu-id="2d2bf-180">c.</span></span> <span data-ttu-id="2d2bf-181">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="2d2bf-181">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="2d2bf-182">d.</span><span class="sxs-lookup"><span data-stu-id="2d2bf-182">d.</span></span> <span data-ttu-id="2d2bf-183">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="2d2bf-183">Click **Create**.</span></span> 


### <a name="creating-a-fuze-test-user"></a><span data-ttu-id="2d2bf-184">Fuze tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="2d2bf-184">Creating a Fuze test user</span></span>

<span data-ttu-id="2d2bf-185">Fuze alkalmazás teljes csak támogatja idő felhasználói kiépítését, így a felhasználókat a rendszer létrehozza automatikusan, ha azok bejelentkezés.</span><span class="sxs-lookup"><span data-stu-id="2d2bf-185">Fuze application supports full Just in time user provision, so users will get created automatically when they sign-in.</span></span> <span data-ttu-id="2d2bf-186">A további tisztázása, lépjen kapcsolatba az Fuze [támogatja](https://www.fuze.com/support).</span><span class="sxs-lookup"><span data-stu-id="2d2bf-186">For any other clarification, please contact Fuze [support](https://www.fuze.com/support).</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="2d2bf-187">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="2d2bf-187">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="2d2bf-188">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés saját hozzáférés tooFuze megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="2d2bf-188">In this section, you enable Britta Simon toouse Azure single sign-on by granting her access tooFuze.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="2d2bf-190">**tooassign Britta Simon tooFuze, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="2d2bf-190">**tooassign Britta Simon tooFuze, perform hello following steps:**</span></span>

1. <span data-ttu-id="2d2bf-191">Hello Azure felügyeleti portálján, nyissa meg a hello alkalmazások megtekintése, majd toohello könyvtár nézetben keresse meg, és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="2d2bf-191">In hello Azure Management portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="2d2bf-193">Hello alkalmazások listában válassza ki a **Fuze**.</span><span class="sxs-lookup"><span data-stu-id="2d2bf-193">In hello applications list, select **Fuze**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-fuze-tutorial/tutorial_fuze_50.png) 

3. <span data-ttu-id="2d2bf-195">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="2d2bf-195">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="2d2bf-197">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="2d2bf-197">Click **Add** button.</span></span> <span data-ttu-id="2d2bf-198">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="2d2bf-198">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="2d2bf-200">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="2d2bf-200">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="2d2bf-201">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="2d2bf-201">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="2d2bf-202">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="2d2bf-202">Click **Assign** button on **Add Assignment** dialog.</span></span>
    

### <a name="testing-single-sign-on"></a><span data-ttu-id="2d2bf-203">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="2d2bf-203">Testing single sign-on</span></span>

<span data-ttu-id="2d2bf-204">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="2d2bf-204">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="2d2bf-205">Ha a hozzáférési Panel hello hello Fuze csempe gombra kattint, automatikusan bejelentkezett tooyour Fuze alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="2d2bf-205">When you click hello Fuze tile in hello Access Panel, you should get automatically signed-on tooyour Fuze application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="2d2bf-206">További források</span><span class="sxs-lookup"><span data-stu-id="2d2bf-206">Additional resources</span></span>

* [<span data-ttu-id="2d2bf-207">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="2d2bf-207">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="2d2bf-208">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="2d2bf-208">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-fuze-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-fuze-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-fuze-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-fuze-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-fuze-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-fuze-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-fuze-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-fuze-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-fuze-tutorial/tutorial_general_203.png