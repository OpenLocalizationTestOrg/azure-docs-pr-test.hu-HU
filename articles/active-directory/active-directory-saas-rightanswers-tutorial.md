---
title: "Oktatóanyag: Azure Active Directoryval integrált RightAnswers |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és RightAnswers között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 7f09e25a-a716-41e1-8ca3-fd00e3d1b8cc
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jeedes
ms.openlocfilehash: 745e7ed5a13291afeed8f48a595e95b27d4b0e58
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-rightanswers"></a><span data-ttu-id="9e1e3-103">Oktatóanyag: Azure Active Directoryval integrált RightAnswers</span><span class="sxs-lookup"><span data-stu-id="9e1e3-103">Tutorial: Azure Active Directory integration with RightAnswers</span></span>

<span data-ttu-id="9e1e3-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate RightAnswers az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="9e1e3-104">In this tutorial, you learn how toointegrate RightAnswers with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="9e1e3-105">RightAnswers integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="9e1e3-105">Integrating RightAnswers with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="9e1e3-106">Megadhatja a hozzáférés tooRightAnswers rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="9e1e3-106">You can control in Azure AD who has access tooRightAnswers</span></span>
- <span data-ttu-id="9e1e3-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooRightAnswers (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="9e1e3-107">You can enable your users tooautomatically get signed-on tooRightAnswers (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="9e1e3-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="9e1e3-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="9e1e3-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="9e1e3-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9e1e3-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="9e1e3-110">Prerequisites</span></span>

<span data-ttu-id="9e1e3-111">az Azure AD integrálása RightAnswers tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="9e1e3-111">tooconfigure Azure AD integration with RightAnswers, you need hello following items:</span></span>

- <span data-ttu-id="9e1e3-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="9e1e3-112">An Azure AD subscription</span></span>
- <span data-ttu-id="9e1e3-113">Egy RightAnswers egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="9e1e3-113">A RightAnswers single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="9e1e3-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="9e1e3-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="9e1e3-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="9e1e3-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="9e1e3-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="9e1e3-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="9e1e3-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="9e1e3-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="9e1e3-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="9e1e3-118">Scenario description</span></span>
<span data-ttu-id="9e1e3-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="9e1e3-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="9e1e3-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="9e1e3-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="9e1e3-121">Hello gyűjteményből RightAnswers hozzáadása</span><span class="sxs-lookup"><span data-stu-id="9e1e3-121">Adding RightAnswers from hello gallery</span></span>
2. <span data-ttu-id="9e1e3-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="9e1e3-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-rightanswers-from-hello-gallery"></a><span data-ttu-id="9e1e3-123">Hello gyűjteményből RightAnswers hozzáadása</span><span class="sxs-lookup"><span data-stu-id="9e1e3-123">Adding RightAnswers from hello gallery</span></span>
<span data-ttu-id="9e1e3-124">tooconfigure hello integrációja RightAnswers az Azure AD-be, meg kell tooadd RightAnswers hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="9e1e3-124">tooconfigure hello integration of RightAnswers into Azure AD, you need tooadd RightAnswers from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="9e1e3-125">**tooadd RightAnswers hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="9e1e3-125">**tooadd RightAnswers from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="9e1e3-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="9e1e3-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="9e1e3-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="9e1e3-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="9e1e3-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="9e1e3-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="9e1e3-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="9e1e3-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="9e1e3-133">Hello keresési mezőbe, írja be a **RightAnswers**.</span><span class="sxs-lookup"><span data-stu-id="9e1e3-133">In hello search box, type **RightAnswers**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-rightanswers-tutorial/tutorial_rightanswers_search.png)

5. <span data-ttu-id="9e1e3-135">A hello eredmények panelen válassza ki a **RightAnswers**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="9e1e3-135">In hello results panel, select **RightAnswers**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-rightanswers-tutorial/tutorial_rightanswers_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="9e1e3-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="9e1e3-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="9e1e3-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján RightAnswers</span><span class="sxs-lookup"><span data-stu-id="9e1e3-138">In this section, you configure and test Azure AD single sign-on with RightAnswers based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="9e1e3-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó RightAnswers tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="9e1e3-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in RightAnswers is tooa user in Azure AD.</span></span> <span data-ttu-id="9e1e3-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello RightAnswers közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="9e1e3-140">In other words, a link relationship between an Azure AD user and hello related user in RightAnswers needs toobe established.</span></span>

<span data-ttu-id="9e1e3-141">RightAnswers, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="9e1e3-141">In RightAnswers, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="9e1e3-142">tooconfigure és az Azure AD az egyszeri bejelentkezés RightAnswers-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="9e1e3-142">tooconfigure and test Azure AD single sign-on with RightAnswers, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="9e1e3-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="9e1e3-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="9e1e3-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="9e1e3-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="9e1e3-145">**[RightAnswers tesztfelhasználó létrehozása](#creating-a-rightanswers-test-user)**  -toohave egy megfelelője a Britta Simon a RightAnswers, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="9e1e3-145">**[Creating a RightAnswers test user](#creating-a-rightanswers-test-user)** - toohave a counterpart of Britta Simon in RightAnswers that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="9e1e3-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="9e1e3-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="9e1e3-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="9e1e3-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="9e1e3-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="9e1e3-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="9e1e3-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az RightAnswers alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="9e1e3-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your RightAnswers application.</span></span>

<span data-ttu-id="9e1e3-150">**az Azure AD tooconfigure egyszeri bejelentkezést a RightAnswers, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="9e1e3-150">**tooconfigure Azure AD single sign-on with RightAnswers, perform hello following steps:**</span></span>

1. <span data-ttu-id="9e1e3-151">Az Azure portál, a hello hello **RightAnswers** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="9e1e3-151">In hello Azure portal, on hello **RightAnswers** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="9e1e3-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="9e1e3-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-rightanswers-tutorial/tutorial_rightanswers_samlbase.png)

3. <span data-ttu-id="9e1e3-155">A hello **RightAnswers tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="9e1e3-155">On hello **RightAnswers Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-rightanswers-tutorial/tutorial_rightanswers_url.png)

    <span data-ttu-id="9e1e3-157">a.</span><span class="sxs-lookup"><span data-stu-id="9e1e3-157">a.</span></span> <span data-ttu-id="9e1e3-158">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<subdomain>.rightanswers.com/portal/ss/`</span><span class="sxs-lookup"><span data-stu-id="9e1e3-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<subdomain>.rightanswers.com/portal/ss/`</span></span>

    <span data-ttu-id="9e1e3-159">b.</span><span class="sxs-lookup"><span data-stu-id="9e1e3-159">b.</span></span> <span data-ttu-id="9e1e3-160">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<subdomain>.rightanswers.com:<identifier>/portal`</span><span class="sxs-lookup"><span data-stu-id="9e1e3-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<subdomain>.rightanswers.com:<identifier>/portal`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="9e1e3-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="9e1e3-161">These values are not real.</span></span> <span data-ttu-id="9e1e3-162">Frissítse a bejelentkezési URL-cím és azonosító a hello tényleges értékek.</span><span class="sxs-lookup"><span data-stu-id="9e1e3-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="9e1e3-163">Ügyfél [RightAnswers ügyfél-támogatási csoport](https://www.rightanswers.com/contact-us/) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="9e1e3-163">Contact [RightAnswers Client support team](https://www.rightanswers.com/contact-us/) tooget these values.</span></span> 
 
4. <span data-ttu-id="9e1e3-164">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="9e1e3-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-rightanswers-tutorial/tutorial_rightanswers_certificate.png) 

5. <span data-ttu-id="9e1e3-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="9e1e3-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-rightanswers-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="9e1e3-168">tooconfigure egyszeri bejelentkezést a **RightAnswers** oldalon kell letöltött toosend hello **metaadatainak XML-kódja** túl[RightAnswers támogatási csoport](https://www.rightanswers.com/contact-us/).</span><span class="sxs-lookup"><span data-stu-id="9e1e3-168">tooconfigure single sign-on on **RightAnswers** side, you need toosend hello downloaded **Metadata XML** too[RightAnswers support team](https://www.rightanswers.com/contact-us/).</span></span>

    >[!NOTE]
    ><span data-ttu-id="9e1e3-169">A RightAnswers támogatási csoport rendelkezik toodo hello tényleges SSO konfigurációval.</span><span class="sxs-lookup"><span data-stu-id="9e1e3-169">Your RightAnswers support team has toodo hello actual SSO configuration.</span></span>
    ><span data-ttu-id="9e1e3-170">Ha egyszeri bejelentkezés engedélyezve van az előfizetés értesítést kap.</span><span class="sxs-lookup"><span data-stu-id="9e1e3-170">You will get a notification when SSO has been enabled for your subscription.</span></span>

> [!TIP]
> <span data-ttu-id="9e1e3-171">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="9e1e3-171">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="9e1e3-172">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="9e1e3-172">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="9e1e3-173">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="9e1e3-173">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="9e1e3-174">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="9e1e3-174">Creating an Azure AD test user</span></span>
<span data-ttu-id="9e1e3-175">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="9e1e3-175">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="9e1e3-177">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="9e1e3-177">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="9e1e3-178">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="9e1e3-178">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-rightanswers-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="9e1e3-180">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="9e1e3-180">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-rightanswers-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="9e1e3-182">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="9e1e3-182">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-rightanswers-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="9e1e3-184">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="9e1e3-184">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-rightanswers-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="9e1e3-186">a.</span><span class="sxs-lookup"><span data-stu-id="9e1e3-186">a.</span></span> <span data-ttu-id="9e1e3-187">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="9e1e3-187">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="9e1e3-188">b.</span><span class="sxs-lookup"><span data-stu-id="9e1e3-188">b.</span></span> <span data-ttu-id="9e1e3-189">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="9e1e3-189">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="9e1e3-190">c.</span><span class="sxs-lookup"><span data-stu-id="9e1e3-190">c.</span></span> <span data-ttu-id="9e1e3-191">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="9e1e3-191">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="9e1e3-192">d.</span><span class="sxs-lookup"><span data-stu-id="9e1e3-192">d.</span></span> <span data-ttu-id="9e1e3-193">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="9e1e3-193">Click **Create**.</span></span>
 
### <a name="creating-a-rightanswers-test-user"></a><span data-ttu-id="9e1e3-194">RightAnswers tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="9e1e3-194">Creating a RightAnswers test user</span></span>

<span data-ttu-id="9e1e3-195">az Azure AD tooenable felhasználók toolog a tooRightAnswers, akkor ki kell építenie RightAnswers be.</span><span class="sxs-lookup"><span data-stu-id="9e1e3-195">tooenable Azure AD users toolog in tooRightAnswers, they must be provisioned into RightAnswers.</span></span> <span data-ttu-id="9e1e3-196">RightAnswers kiépítés esetén egy automatizált tevékenység, nincs teendő elem meg.</span><span class="sxs-lookup"><span data-stu-id="9e1e3-196">When RightAnswers, provisioning is an automated task so there is no action item for you.</span></span>

<span data-ttu-id="9e1e3-197">Felhasználók automatikusan létrejönnek szükség hello első egyszeri bejelentkezési kísérlet során.</span><span class="sxs-lookup"><span data-stu-id="9e1e3-197">Users are automatically created if necessary during hello first single sign-on attempt.</span></span>

>[!NOTE]
><span data-ttu-id="9e1e3-198">Bármely más RightAnswers felhasználói fiók létrehozása eszközök vagy RightAnswers tooprovision által nyújtott API-k AAD felhasználói fiókokat.</span><span class="sxs-lookup"><span data-stu-id="9e1e3-198">You can use any other RightAnswers user account creation tools or APIs provided by RightAnswers tooprovision AAD user accounts.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="9e1e3-199">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="9e1e3-199">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="9e1e3-200">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooRightAnswers megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="9e1e3-200">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooRightAnswers.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="9e1e3-202">**tooassign Britta Simon tooRightAnswers, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="9e1e3-202">**tooassign Britta Simon tooRightAnswers, perform hello following steps:**</span></span>

1. <span data-ttu-id="9e1e3-203">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="9e1e3-203">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="9e1e3-205">Hello alkalmazások listában válassza ki a **RightAnswers**.</span><span class="sxs-lookup"><span data-stu-id="9e1e3-205">In hello applications list, select **RightAnswers**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-rightanswers-tutorial/tutorial_rightanswers_app.png) 

3. <span data-ttu-id="9e1e3-207">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="9e1e3-207">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="9e1e3-209">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="9e1e3-209">Click **Add** button.</span></span> <span data-ttu-id="9e1e3-210">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="9e1e3-210">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="9e1e3-212">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="9e1e3-212">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="9e1e3-213">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="9e1e3-213">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="9e1e3-214">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="9e1e3-214">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="9e1e3-215">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="9e1e3-215">Testing single sign-on</span></span>

<span data-ttu-id="9e1e3-216">Ha tootest az egyszeri Bejelentkezést a beállításokat, nyissa meg a hozzáférési Panel hello.</span><span class="sxs-lookup"><span data-stu-id="9e1e3-216">If you want tootest your SSO settings, open hello Access Panel.</span></span> <span data-ttu-id="9e1e3-217">További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="9e1e3-217">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>
## <a name="additional-resources"></a><span data-ttu-id="9e1e3-218">További források</span><span class="sxs-lookup"><span data-stu-id="9e1e3-218">Additional resources</span></span>

* [<span data-ttu-id="9e1e3-219">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="9e1e3-219">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="9e1e3-220">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="9e1e3-220">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-rightanswers-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-rightanswers-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-rightanswers-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-rightanswers-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-rightanswers-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-rightanswers-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-rightanswers-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-rightanswers-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-rightanswers-tutorial/tutorial_general_203.png

