---
title: "Oktatóanyag: Azure Active Directoryval integrált MobileXpense |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és MobileXpense között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: e649fc4e-3e15-4948-b977-00bfe9f7db13
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/25/2017
ms.author: jeedes
ms.openlocfilehash: b9d109f9d4244f8a7eb8b49b0d980cd3df0fc59d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-mobilexpense"></a><span data-ttu-id="62a17-103">Oktatóanyag: Azure Active Directoryval integrált MobileXpense</span><span class="sxs-lookup"><span data-stu-id="62a17-103">Tutorial: Azure Active Directory integration with MobileXpense</span></span>

<span data-ttu-id="62a17-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate MobileXpense az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="62a17-104">In this tutorial, you learn how toointegrate MobileXpense with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="62a17-105">MobileXpense integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="62a17-105">Integrating MobileXpense with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="62a17-106">Megadhatja a hozzáférés tooMobileXpense rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="62a17-106">You can control in Azure AD who has access tooMobileXpense</span></span>
- <span data-ttu-id="62a17-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooMobileXpense (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="62a17-107">You can enable your users tooautomatically get signed-on tooMobileXpense (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="62a17-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="62a17-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="62a17-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="62a17-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="62a17-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="62a17-110">Prerequisites</span></span>

<span data-ttu-id="62a17-111">az Azure AD integrálása MobileXpense tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="62a17-111">tooconfigure Azure AD integration with MobileXpense, you need hello following items:</span></span>

- <span data-ttu-id="62a17-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="62a17-112">An Azure AD subscription</span></span>
- <span data-ttu-id="62a17-113">Egy MobileXpense egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="62a17-113">A MobileXpense single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="62a17-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="62a17-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="62a17-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="62a17-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="62a17-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="62a17-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="62a17-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="62a17-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="62a17-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="62a17-118">Scenario description</span></span>
<span data-ttu-id="62a17-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="62a17-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="62a17-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="62a17-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="62a17-121">Hello gyűjteményből MobileXpense hozzáadása</span><span class="sxs-lookup"><span data-stu-id="62a17-121">Adding MobileXpense from hello gallery</span></span>
2. <span data-ttu-id="62a17-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="62a17-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-mobilexpense-from-hello-gallery"></a><span data-ttu-id="62a17-123">Hello gyűjteményből MobileXpense hozzáadása</span><span class="sxs-lookup"><span data-stu-id="62a17-123">Adding MobileXpense from hello gallery</span></span>
<span data-ttu-id="62a17-124">tooconfigure hello integrációja MobileXpense az Azure AD-be, meg kell tooadd MobileXpense hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="62a17-124">tooconfigure hello integration of MobileXpense into Azure AD, you need tooadd MobileXpense from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="62a17-125">**tooadd MobileXpense hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="62a17-125">**tooadd MobileXpense from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="62a17-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="62a17-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="62a17-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="62a17-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="62a17-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="62a17-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="62a17-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="62a17-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="62a17-133">Hello keresési mezőbe, írja be a **MobileXpense**.</span><span class="sxs-lookup"><span data-stu-id="62a17-133">In hello search box, type **MobileXpense**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-mobilexpense-tutorial/tutorial_mobilexpense_search.png)

5. <span data-ttu-id="62a17-135">A hello eredmények panelen válassza ki a **MobileXpense**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="62a17-135">In hello results panel, select **MobileXpense**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-mobilexpense-tutorial/tutorial_mobilexpense_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="62a17-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="62a17-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="62a17-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján MobileXpense</span><span class="sxs-lookup"><span data-stu-id="62a17-138">In this section, you configure and test Azure AD single sign-on with MobileXpense based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="62a17-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó MobileXpense tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="62a17-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in MobileXpense is tooa user in Azure AD.</span></span> <span data-ttu-id="62a17-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello MobileXpense közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="62a17-140">In other words, a link relationship between an Azure AD user and hello related user in MobileXpense needs toobe established.</span></span>

<span data-ttu-id="62a17-141">MobileXpense, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="62a17-141">In MobileXpense, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="62a17-142">tooconfigure és az Azure AD az egyszeri bejelentkezés MobileXpense-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="62a17-142">tooconfigure and test Azure AD single sign-on with MobileXpense, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="62a17-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="62a17-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="62a17-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="62a17-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="62a17-145">**[MobileXpense tesztfelhasználó létrehozása](#creating-a-mobilexpense-test-user)**  -toohave egy megfelelője a Britta Simon a MobileXpense, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="62a17-145">**[Creating a MobileXpense test user](#creating-a-mobilexpense-test-user)** - toohave a counterpart of Britta Simon in MobileXpense that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="62a17-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="62a17-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="62a17-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="62a17-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="62a17-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="62a17-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="62a17-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az MobileXpense alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="62a17-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your MobileXpense application.</span></span>

<span data-ttu-id="62a17-150">**az Azure AD tooconfigure egyszeri bejelentkezést a MobileXpense, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="62a17-150">**tooconfigure Azure AD single sign-on with MobileXpense, perform hello following steps:**</span></span>

1. <span data-ttu-id="62a17-151">Az Azure portál, a hello hello **MobileXpense** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="62a17-151">In hello Azure portal, on hello **MobileXpense** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="62a17-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="62a17-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-mobilexpense-tutorial/tutorial_mobilexpense_samlbase.png)

3. <span data-ttu-id="62a17-155">A hello **MobileXpense tartomány és az URL-címek** szakaszban, ha tooconfigure hello alkalmazás **IDP** kezdeményezett mód:</span><span class="sxs-lookup"><span data-stu-id="62a17-155">On hello **MobileXpense Domain and URLs** section, If you wish tooconfigure hello application in **IDP** initiated mode:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-mobilexpense-tutorial/tutorial_mobilexpense_url11.png)

    <span data-ttu-id="62a17-157">A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<sub domain>.mobilexpense.com/SSO/SAML20/SAML/AssertionConsumerService.aspx`</span><span class="sxs-lookup"><span data-stu-id="62a17-157">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<sub domain>.mobilexpense.com/SSO/SAML20/SAML/AssertionConsumerService.aspx`</span></span>

4. <span data-ttu-id="62a17-158">Ellenőrizze **megjelenítése speciális URL-beállításainak**, ha tooconfigure hello alkalmazás **SP** kezdeményezett mód:</span><span class="sxs-lookup"><span data-stu-id="62a17-158">Check **Show advanced URL settings**, If you wish tooconfigure hello application in **SP** initiated mode:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-mobilexpense-tutorial/tutorial_mobilexpense_url22.png)

<span data-ttu-id="62a17-160">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával adja meg:`https://<sub domain>.mobilexpense.com/<customername>`</span><span class="sxs-lookup"><span data-stu-id="62a17-160">In hello **Sign-on URL** textbox, type a URL using hello following pattern:: `https://<sub domain>.mobilexpense.com/<customername>`</span></span>

> [!NOTE] 
> <span data-ttu-id="62a17-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="62a17-161">These values are not real.</span></span> <span data-ttu-id="62a17-162">Ezek az értékek frissítése hello tényleges válasz és bejelentkezési URL-címe.</span><span class="sxs-lookup"><span data-stu-id="62a17-162">Update these values with hello actual Reply URL and Sign-On URL.</span></span> <span data-ttu-id="62a17-163">Ügyfél [MobileXpense ügyfél-támogatási csoport](http://www.mobilexpense.net/contact) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="62a17-163">Contact [MobileXpense Client support team](http://www.mobilexpense.net/contact) tooget these values.</span></span> 

5. <span data-ttu-id="62a17-164">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="62a17-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-mobilexpense-tutorial/tutorial_mobilexpense_certificate.png) 

6. <span data-ttu-id="62a17-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="62a17-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-mobilexpense-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="62a17-168">tooconfigure egyszeri bejelentkezést a **MobileXpense** oldalon kell letöltött toosend hello **metaadatainak XML-kódja** túl[MobileXpense támogatási csoport](http://www.mobilexpense.net/contact).</span><span class="sxs-lookup"><span data-stu-id="62a17-168">tooconfigure single sign-on on **MobileXpense** side, you need toosend hello downloaded **Metadata XML** too[MobileXpense support team](http://www.mobilexpense.net/contact).</span></span>

> [!TIP]
> <span data-ttu-id="62a17-169">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="62a17-169">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="62a17-170">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="62a17-170">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="62a17-171">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="62a17-171">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="62a17-172">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="62a17-172">Creating an Azure AD test user</span></span>
<span data-ttu-id="62a17-173">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="62a17-173">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="62a17-175">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="62a17-175">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="62a17-176">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="62a17-176">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-mobilexpense-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="62a17-178">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="62a17-178">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-mobilexpense-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="62a17-180">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="62a17-180">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-mobilexpense-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="62a17-182">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="62a17-182">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-mobilexpense-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="62a17-184">a.</span><span class="sxs-lookup"><span data-stu-id="62a17-184">a.</span></span> <span data-ttu-id="62a17-185">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="62a17-185">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="62a17-186">b.</span><span class="sxs-lookup"><span data-stu-id="62a17-186">b.</span></span> <span data-ttu-id="62a17-187">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="62a17-187">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="62a17-188">c.</span><span class="sxs-lookup"><span data-stu-id="62a17-188">c.</span></span> <span data-ttu-id="62a17-189">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="62a17-189">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="62a17-190">d.</span><span class="sxs-lookup"><span data-stu-id="62a17-190">d.</span></span> <span data-ttu-id="62a17-191">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="62a17-191">Click **Create**.</span></span>
 
### <a name="creating-a-mobilexpense-test-user"></a><span data-ttu-id="62a17-192">MobileXpense tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="62a17-192">Creating a MobileXpense test user</span></span>

<span data-ttu-id="62a17-193">Ebben a szakaszban egy MobileXpense Britta Simon nevű felhasználót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="62a17-193">In this section, you create a user called Britta Simon in MobileXpense.</span></span> <span data-ttu-id="62a17-194">a [MobileXpense támogatási csoport](http://www.mobilexpense.net/contact) felhasználót is hozzáadhat hello hello MobileXpense platform.</span><span class="sxs-lookup"><span data-stu-id="62a17-194">work with [MobileXpense support team](http://www.mobilexpense.net/contact) to add hello users in hello MobileXpense platform.</span></span> <span data-ttu-id="62a17-195">Felhasználók kell létrehoznia és aktiválni az egyszeri bejelentkezés használata előtt.</span><span class="sxs-lookup"><span data-stu-id="62a17-195">Users must be created and activated before you use single sign-on.</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="62a17-196">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="62a17-196">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="62a17-197">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooMobileXpense megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="62a17-197">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooMobileXpense.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="62a17-199">**tooassign Britta Simon tooMobileXpense, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="62a17-199">**tooassign Britta Simon tooMobileXpense, perform hello following steps:**</span></span>

1. <span data-ttu-id="62a17-200">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="62a17-200">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="62a17-202">Hello alkalmazások listában válassza ki a **MobileXpense**.</span><span class="sxs-lookup"><span data-stu-id="62a17-202">In hello applications list, select **MobileXpense**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-mobilexpense-tutorial/tutorial_mobilexpense_app.png) 

3. <span data-ttu-id="62a17-204">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="62a17-204">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="62a17-206">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="62a17-206">Click **Add** button.</span></span> <span data-ttu-id="62a17-207">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="62a17-207">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="62a17-209">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="62a17-209">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="62a17-210">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="62a17-210">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="62a17-211">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="62a17-211">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="62a17-212">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="62a17-212">Testing single sign-on</span></span>

<span data-ttu-id="62a17-213">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="62a17-213">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="62a17-214">Ha a hozzáférési Panel hello hello MobileXpense csempe gombra kattint, automatikusan bejelentkezett tooyour MobileXpense alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="62a17-214">When you click hello MobileXpense tile in hello Access Panel, you should get automatically signed-on tooyour MobileXpense application.</span></span>
<span data-ttu-id="62a17-215">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="62a17-215">For more information about the Access Panel, see [introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="62a17-216">További források</span><span class="sxs-lookup"><span data-stu-id="62a17-216">Additional resources</span></span>

* [<span data-ttu-id="62a17-217">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="62a17-217">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="62a17-218">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="62a17-218">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-mobilexpense-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-mobilexpense-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-mobilexpense-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-mobilexpense-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-mobilexpense-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-mobilexpense-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-mobilexpense-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-mobilexpense-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-mobilexpense-tutorial/tutorial_general_203.png

