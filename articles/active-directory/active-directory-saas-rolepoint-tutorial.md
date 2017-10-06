---
title: "Oktatóanyag: Azure Active Directoryval integrált RolePoint |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és RolePoint között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 68d37f40-15da-45f5-a9e1-d53f78e786d1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/27/2017
ms.author: jeedes
ms.openlocfilehash: 8629dd87c17d44ab89251ebbd19156c6d6cbedc5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-rolepoint"></a><span data-ttu-id="01747-103">Oktatóanyag: Azure Active Directoryval integrált RolePoint</span><span class="sxs-lookup"><span data-stu-id="01747-103">Tutorial: Azure Active Directory integration with RolePoint</span></span>

<span data-ttu-id="01747-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate RolePoint az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="01747-104">In this tutorial, you learn how toointegrate RolePoint with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="01747-105">RolePoint integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="01747-105">Integrating RolePoint with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="01747-106">Megadhatja a hozzáférés tooRolePoint rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="01747-106">You can control in Azure AD who has access tooRolePoint</span></span>
- <span data-ttu-id="01747-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooRolePoint (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="01747-107">You can enable your users tooautomatically get signed-on tooRolePoint (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="01747-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="01747-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="01747-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="01747-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="01747-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="01747-110">Prerequisites</span></span>

<span data-ttu-id="01747-111">az Azure AD integrálása RolePoint tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="01747-111">tooconfigure Azure AD integration with RolePoint, you need hello following items:</span></span>

- <span data-ttu-id="01747-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="01747-112">An Azure AD subscription</span></span>
- <span data-ttu-id="01747-113">Egy RolePoint egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="01747-113">A RolePoint single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="01747-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="01747-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="01747-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="01747-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="01747-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="01747-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="01747-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="01747-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="01747-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="01747-118">Scenario description</span></span>
<span data-ttu-id="01747-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="01747-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="01747-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="01747-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="01747-121">Hello gyűjteményből RolePoint hozzáadása</span><span class="sxs-lookup"><span data-stu-id="01747-121">Adding RolePoint from hello gallery</span></span>
2. <span data-ttu-id="01747-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="01747-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-rolepoint-from-hello-gallery"></a><span data-ttu-id="01747-123">Hello gyűjteményből RolePoint hozzáadása</span><span class="sxs-lookup"><span data-stu-id="01747-123">Adding RolePoint from hello gallery</span></span>
<span data-ttu-id="01747-124">tooconfigure hello integrációja RolePoint az Azure AD-be, meg kell tooadd RolePoint hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="01747-124">tooconfigure hello integration of RolePoint into Azure AD, you need tooadd RolePoint from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="01747-125">**tooadd RolePoint hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="01747-125">**tooadd RolePoint from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="01747-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="01747-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="01747-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="01747-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="01747-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="01747-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="01747-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="01747-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="01747-133">Hello keresési mezőbe, írja be a **RolePoint**.</span><span class="sxs-lookup"><span data-stu-id="01747-133">In hello search box, type **RolePoint**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-rolepoint-tutorial/tutorial_rolepoint_search.png)

5. <span data-ttu-id="01747-135">A hello eredmények panelen válassza ki a **RolePoint**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="01747-135">In hello results panel, select **RolePoint**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-rolepoint-tutorial/tutorial_rolepoint_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="01747-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="01747-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="01747-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján RolePoint</span><span class="sxs-lookup"><span data-stu-id="01747-138">In this section, you configure and test Azure AD single sign-on with RolePoint based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="01747-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó RolePoint tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="01747-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in RolePoint is tooa user in Azure AD.</span></span> <span data-ttu-id="01747-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello RolePoint közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="01747-140">In other words, a link relationship between an Azure AD user and hello related user in RolePoint needs toobe established.</span></span>

<span data-ttu-id="01747-141">Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** a RolePoint.</span><span class="sxs-lookup"><span data-stu-id="01747-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in RolePoint.</span></span>

<span data-ttu-id="01747-142">tooconfigure és az Azure AD az egyszeri bejelentkezés RolePoint-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="01747-142">tooconfigure and test Azure AD single sign-on with RolePoint, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="01747-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="01747-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="01747-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="01747-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="01747-145">**[RolePoint tesztfelhasználó létrehozása](#creating-a-rolepoint-test-user)**  -toohave egy megfelelője a Britta Simon a RolePoint, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="01747-145">**[Creating a RolePoint test user](#creating-a-rolepoint-test-user)** - toohave a counterpart of Britta Simon in RolePoint that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="01747-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="01747-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="01747-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="01747-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="01747-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="01747-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="01747-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az RolePoint alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="01747-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your RolePoint application.</span></span>

<span data-ttu-id="01747-150">**az Azure AD tooconfigure egyszeri bejelentkezést a RolePoint, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="01747-150">**tooconfigure Azure AD single sign-on with RolePoint, perform hello following steps:**</span></span>

1. <span data-ttu-id="01747-151">Az Azure portál, a hello hello **RolePoint** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="01747-151">In hello Azure portal, on hello **RolePoint** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="01747-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="01747-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-rolepoint-tutorial/tutorial_rolepoint_samlbase.png)

3. <span data-ttu-id="01747-155">A hello **RolePoint tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="01747-155">On hello **RolePoint Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-rolepoint-tutorial/tutorial_rolepoint_url.png)

    <span data-ttu-id="01747-157">a.</span><span class="sxs-lookup"><span data-stu-id="01747-157">a.</span></span> <span data-ttu-id="01747-158">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<subdomain>.rolepoint.com/login`</span><span class="sxs-lookup"><span data-stu-id="01747-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<subdomain>.rolepoint.com/login`</span></span>
    
    <span data-ttu-id="01747-159">b.</span><span class="sxs-lookup"><span data-stu-id="01747-159">b.</span></span> <span data-ttu-id="01747-160">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://app.rolepoint.com/<instancename>`</span><span class="sxs-lookup"><span data-stu-id="01747-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://app.rolepoint.com/<instancename>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="01747-161">Ezek az értékek nincsenek hello valós.</span><span class="sxs-lookup"><span data-stu-id="01747-161">These values are not hello real.</span></span> <span data-ttu-id="01747-162">Frissítheti ezeket az értékeket a hello tényleges bejelentkezési URL-cím és azonosítója.</span><span class="sxs-lookup"><span data-stu-id="01747-162">Update these values with hello actual Sign-on URL and Identifier.</span></span> <span data-ttu-id="01747-163">Itt javasoljuk, hogy toouse hello egyedi érték karakterlánc hello Identifier.Contact [RolePoint támogatási csoport](mailto:info@rolepoint.com) tooget hello érték.</span><span class="sxs-lookup"><span data-stu-id="01747-163">Here we suggest you toouse hello unique value of string in hello Identifier.Contact [RolePoint support team](mailto:info@rolepoint.com) tooget hello value.</span></span> 
 
4. <span data-ttu-id="01747-164">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="01747-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-rolepoint-tutorial/tutorial_rolepoint_certificate.png) 

5. <span data-ttu-id="01747-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="01747-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-rolepoint-tutorial/tutorial_general_400.png)


6. <span data-ttu-id="01747-168">tooconfigure egyszeri bejelentkezést a **RolePoint** oldalon kell letöltött toosend hello **metaadatainak XML-kódja** túl[RolePoint támogatási csoport](mailto:info@rolepoint.com).</span><span class="sxs-lookup"><span data-stu-id="01747-168">tooconfigure single sign-on on **RolePoint** side, you need toosend hello downloaded **Metadata XML** too[RolePoint support team](mailto:info@rolepoint.com).</span></span>

> [!TIP]
> <span data-ttu-id="01747-169">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="01747-169">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="01747-170">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="01747-170">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="01747-171">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="01747-171">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="01747-172">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="01747-172">Creating an Azure AD test user</span></span>
<span data-ttu-id="01747-173">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="01747-173">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="01747-175">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="01747-175">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="01747-176">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="01747-176">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-rolepoint-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="01747-178">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="01747-178">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-rolepoint-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="01747-180">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="01747-180">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-rolepoint-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="01747-182">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="01747-182">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-rolepoint-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="01747-184">a.</span><span class="sxs-lookup"><span data-stu-id="01747-184">a.</span></span> <span data-ttu-id="01747-185">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="01747-185">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="01747-186">b.</span><span class="sxs-lookup"><span data-stu-id="01747-186">b.</span></span> <span data-ttu-id="01747-187">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="01747-187">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="01747-188">c.</span><span class="sxs-lookup"><span data-stu-id="01747-188">c.</span></span> <span data-ttu-id="01747-189">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="01747-189">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="01747-190">d.</span><span class="sxs-lookup"><span data-stu-id="01747-190">d.</span></span> <span data-ttu-id="01747-191">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="01747-191">Click **Create**.</span></span>
 
### <a name="creating-a-rolepoint-test-user"></a><span data-ttu-id="01747-192">RolePoint tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="01747-192">Creating a RolePoint test user</span></span>

<span data-ttu-id="01747-193">Ebben a szakaszban egy RolePoint Britta Simon nevű felhasználót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="01747-193">In this section, you create a user called Britta Simon in RolePoint.</span></span> <span data-ttu-id="01747-194">Együttműködve [RolePoint támogatási csoport](mailto:info@rolepoint.com) tooadd hello felhasználók hello RolePoint platform.</span><span class="sxs-lookup"><span data-stu-id="01747-194">Work with [RolePoint support team](mailto:info@rolepoint.com) tooadd hello users in hello RolePoint platform.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="01747-195">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="01747-195">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="01747-196">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooRolePoint megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="01747-196">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooRolePoint.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="01747-198">**tooassign Britta Simon tooRolePoint, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="01747-198">**tooassign Britta Simon tooRolePoint, perform hello following steps:**</span></span>

1. <span data-ttu-id="01747-199">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="01747-199">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="01747-201">Hello alkalmazások listában válassza ki a **RolePoint**.</span><span class="sxs-lookup"><span data-stu-id="01747-201">In hello applications list, select **RolePoint**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-rolepoint-tutorial/tutorial_rolepoint_app.png) 

3. <span data-ttu-id="01747-203">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="01747-203">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="01747-205">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="01747-205">Click **Add** button.</span></span> <span data-ttu-id="01747-206">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="01747-206">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="01747-208">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="01747-208">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="01747-209">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="01747-209">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="01747-210">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="01747-210">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="01747-211">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="01747-211">Testing single sign-on</span></span>

<span data-ttu-id="01747-212">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="01747-212">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="01747-213">Ha a hozzáférési Panel hello hello RolePoint csempe gombra kattint, automatikusan bejelentkezett tooyour RolePoint alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="01747-213">When you click hello RolePoint tile in hello Access Panel, you should get automatically signed-on tooyour RolePoint application.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="01747-214">További források</span><span class="sxs-lookup"><span data-stu-id="01747-214">Additional resources</span></span>

* [<span data-ttu-id="01747-215">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="01747-215">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="01747-216">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="01747-216">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-rolepoint-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-rolepoint-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-rolepoint-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-rolepoint-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-rolepoint-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-rolepoint-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-rolepoint-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-rolepoint-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-rolepoint-tutorial/tutorial_general_203.png

