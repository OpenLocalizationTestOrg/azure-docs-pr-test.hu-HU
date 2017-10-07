---
title: "Oktatóanyag: Azure Active Directoryval integrált Jostle |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Jostle között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 9ca4ca1f-8f68-4225-81a6-1666b486d6a8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes
ms.openlocfilehash: 617375d8f9e1cebcdb28450fc8d0ad8af99d7b22
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-jostle"></a><span data-ttu-id="2b69b-103">Oktatóanyag: Azure Active Directoryval integrált Jostle</span><span class="sxs-lookup"><span data-stu-id="2b69b-103">Tutorial: Azure Active Directory integration with Jostle</span></span>

<span data-ttu-id="2b69b-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Jostle az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="2b69b-104">In this tutorial, you learn how toointegrate Jostle with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="2b69b-105">Jostle integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="2b69b-105">Integrating Jostle with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="2b69b-106">Megadhatja a hozzáférés tooJostle rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="2b69b-106">You can control in Azure AD who has access tooJostle</span></span>
- <span data-ttu-id="2b69b-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooJostle (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="2b69b-107">You can enable your users tooautomatically get signed-on tooJostle (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="2b69b-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="2b69b-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="2b69b-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="2b69b-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2b69b-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="2b69b-110">Prerequisites</span></span>

<span data-ttu-id="2b69b-111">Jostle tooconfigure az Azure AD integrálása, a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="2b69b-111">tooconfigure Azure AD integration with Jostle, you need hello following items:</span></span>

- <span data-ttu-id="2b69b-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="2b69b-112">An Azure AD subscription</span></span>
- <span data-ttu-id="2b69b-113">Egy Jostle egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="2b69b-113">A Jostle single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="2b69b-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="2b69b-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="2b69b-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="2b69b-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="2b69b-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="2b69b-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="2b69b-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="2b69b-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="2b69b-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="2b69b-118">Scenario description</span></span>
<span data-ttu-id="2b69b-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="2b69b-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="2b69b-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="2b69b-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="2b69b-121">Hello gyűjteményből Jostle hozzáadása</span><span class="sxs-lookup"><span data-stu-id="2b69b-121">Adding Jostle from hello gallery</span></span>
2. <span data-ttu-id="2b69b-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="2b69b-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-jostle-from-hello-gallery"></a><span data-ttu-id="2b69b-123">Hello gyűjteményből Jostle hozzáadása</span><span class="sxs-lookup"><span data-stu-id="2b69b-123">Adding Jostle from hello gallery</span></span>
<span data-ttu-id="2b69b-124">tooconfigure hello integrációja Jostle az Azure AD-be, meg kell tooadd Jostle hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="2b69b-124">tooconfigure hello integration of Jostle into Azure AD, you need tooadd Jostle from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="2b69b-125">**tooadd Jostle hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="2b69b-125">**tooadd Jostle from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="2b69b-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="2b69b-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="2b69b-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="2b69b-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="2b69b-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="2b69b-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="2b69b-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="2b69b-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="2b69b-133">Hello keresési mezőbe, írja be a **Jostle**.</span><span class="sxs-lookup"><span data-stu-id="2b69b-133">In hello search box, type **Jostle**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-jostle-tutorial/tutorial_jostle_search.png)

5. <span data-ttu-id="2b69b-135">A hello eredmények panelen válassza ki a **Jostle**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="2b69b-135">In hello results panel, select **Jostle**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-jostle-tutorial/tutorial_jostle_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="2b69b-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="2b69b-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="2b69b-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján Jostle</span><span class="sxs-lookup"><span data-stu-id="2b69b-138">In this section, you configure and test Azure AD single sign-on with Jostle based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="2b69b-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Jostle tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="2b69b-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Jostle is tooa user in Azure AD.</span></span> <span data-ttu-id="2b69b-140">Más szóval hivatkozás közötti kapcsolat egy Azure AD-felhasználó és a kapcsolódó felhasználóknak hello Jostle igényeinek toobe létrehozott.</span><span class="sxs-lookup"><span data-stu-id="2b69b-140">In other words, a link relationship between an Azure AD user and hello related user in Jostle needs toobe established.</span></span>

<span data-ttu-id="2b69b-141">Jostle, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="2b69b-141">In Jostle, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="2b69b-142">tooconfigure és az Azure AD az egyszeri bejelentkezés Jostle-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="2b69b-142">tooconfigure and test Azure AD single sign-on with Jostle, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="2b69b-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="2b69b-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="2b69b-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="2b69b-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="2b69b-145">**[Jostle tesztfelhasználó létrehozása](#creating-a-jostle-test-user)**  -toohave egy megfelelője a Britta Simon a Jostle, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="2b69b-145">**[Creating a Jostle test user](#creating-a-jostle-test-user)** - toohave a counterpart of Britta Simon in Jostle that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="2b69b-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="2b69b-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="2b69b-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="2b69b-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="2b69b-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="2b69b-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="2b69b-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Jostle alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="2b69b-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Jostle application.</span></span>

<span data-ttu-id="2b69b-150">**Jostle, az Azure AD az egyszeri bejelentkezés tooconfigure hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="2b69b-150">**tooconfigure Azure AD single sign-on with Jostle, perform hello following steps:**</span></span>

1. <span data-ttu-id="2b69b-151">Az Azure portál, a hello hello **Jostle** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="2b69b-151">In hello Azure portal, on hello **Jostle** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="2b69b-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="2b69b-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-jostle-tutorial/tutorial_jostle_samlbase.png)

3. <span data-ttu-id="2b69b-155">A hello **Jostle tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="2b69b-155">On hello **Jostle Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-jostle-tutorial/tutorial_jostle_url.png)

    <span data-ttu-id="2b69b-157">a.</span><span class="sxs-lookup"><span data-stu-id="2b69b-157">a.</span></span> <span data-ttu-id="2b69b-158">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<tanent name>.jostle.us/jostle-prod/`</span><span class="sxs-lookup"><span data-stu-id="2b69b-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<tanent name>.jostle.us/jostle-prod/`</span></span>

    <span data-ttu-id="2b69b-159">b.</span><span class="sxs-lookup"><span data-stu-id="2b69b-159">b.</span></span> <span data-ttu-id="2b69b-160">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<tanent name>.jostle.us`</span><span class="sxs-lookup"><span data-stu-id="2b69b-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<tanent name>.jostle.us`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="2b69b-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="2b69b-161">These values are not real.</span></span> <span data-ttu-id="2b69b-162">Frissítse a bejelentkezési URL-cím és azonosító a hello tényleges értékek.</span><span class="sxs-lookup"><span data-stu-id="2b69b-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="2b69b-163">Ügyfél [Jostle támogatási csoport](mailto:support@jostle.me) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="2b69b-163">Contact [Jostle support team](mailto:support@jostle.me) tooget these values.</span></span> 
 


4. <span data-ttu-id="2b69b-164">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="2b69b-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-jostle-tutorial/tutorial_jostle_certificate.png) 

5. <span data-ttu-id="2b69b-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="2b69b-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-jostle-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="2b69b-168">tooconfigure egyszeri bejelentkezés Jostle oldalán, toosend kell hello letöltése a metaadatok XML túl[Jostle támogatási csoport](mailto:support@jostle.me).</span><span class="sxs-lookup"><span data-stu-id="2b69b-168">tooconfigure single sign-on on Jostle side, you need toosend hello downloaded metadata XML too[Jostle support team](mailto:support@jostle.me).</span></span> <span data-ttu-id="2b69b-169">Maguk állítják be ezt a beállítást toohave hello SAML SSO kapcsolat mindkét oldalán megfelelően beállítva.</span><span class="sxs-lookup"><span data-stu-id="2b69b-169">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span> 

> [!TIP]
> <span data-ttu-id="2b69b-170">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="2b69b-170">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="2b69b-171">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="2b69b-171">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="2b69b-172">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="2b69b-172">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="2b69b-173">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="2b69b-173">Creating an Azure AD test user</span></span>
<span data-ttu-id="2b69b-174">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="2b69b-174">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="2b69b-176">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="2b69b-176">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="2b69b-177">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="2b69b-177">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-jostle-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="2b69b-179">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="2b69b-179">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-jostle-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="2b69b-181">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="2b69b-181">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-jostle-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="2b69b-183">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="2b69b-183">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-jostle-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="2b69b-185">a.</span><span class="sxs-lookup"><span data-stu-id="2b69b-185">a.</span></span> <span data-ttu-id="2b69b-186">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="2b69b-186">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="2b69b-187">b.</span><span class="sxs-lookup"><span data-stu-id="2b69b-187">b.</span></span> <span data-ttu-id="2b69b-188">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="2b69b-188">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="2b69b-189">c.</span><span class="sxs-lookup"><span data-stu-id="2b69b-189">c.</span></span> <span data-ttu-id="2b69b-190">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="2b69b-190">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="2b69b-191">d.</span><span class="sxs-lookup"><span data-stu-id="2b69b-191">d.</span></span> <span data-ttu-id="2b69b-192">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="2b69b-192">Click **Create**.</span></span>
 
### <a name="creating-a-jostle-test-user"></a><span data-ttu-id="2b69b-193">Jostle tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="2b69b-193">Creating a Jostle test user</span></span>

<span data-ttu-id="2b69b-194">Ebben a szakaszban egy Jostle Britta Simon nevű felhasználót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="2b69b-194">In this section, you create a user called Britta Simon in Jostle.</span></span> <span data-ttu-id="2b69b-195">Ha nem tudja, hogyan tooadd Britta Simon a Jostle, lépjen kapcsolatba a [Jostle támogatási csoport](mailto:support@jostle.me) tooadd hello tesztfelhasználó, és engedélyezze az egyszeri Bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="2b69b-195">If you don't know how tooadd Britta Simon in Jostle, please contact with [Jostle support team](mailto:support@jostle.me) tooadd hello test user and enable SSO.</span></span>

> [!NOTE]
> <span data-ttu-id="2b69b-196">hello Azure Active Directory fióktulajdonos kap egy e-mailt legyen és megfeleljen a fiókjuk egy hivatkozás tooconfirm mielőtt aktívvá válik.</span><span class="sxs-lookup"><span data-stu-id="2b69b-196">hello Azure Active Directory account holder receives an email and follows a link tooconfirm their account before it becomes active.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="2b69b-197">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="2b69b-197">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="2b69b-198">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooJostle megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="2b69b-198">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooJostle.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="2b69b-200">**tooassign Britta Simon tooJostle, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="2b69b-200">**tooassign Britta Simon tooJostle, perform hello following steps:**</span></span>

1. <span data-ttu-id="2b69b-201">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="2b69b-201">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="2b69b-203">Hello alkalmazások listában válassza ki a **Jostle**.</span><span class="sxs-lookup"><span data-stu-id="2b69b-203">In hello applications list, select **Jostle**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-jostle-tutorial/tutorial_jostle_app.png) 

3. <span data-ttu-id="2b69b-205">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="2b69b-205">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="2b69b-207">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="2b69b-207">Click **Add** button.</span></span> <span data-ttu-id="2b69b-208">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="2b69b-208">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="2b69b-210">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="2b69b-210">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="2b69b-211">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="2b69b-211">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="2b69b-212">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="2b69b-212">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="2b69b-213">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="2b69b-213">Testing single sign-on</span></span>

<span data-ttu-id="2b69b-214">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="2b69b-214">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="2b69b-215">Ha a hozzáférési Panel hello hello Jostle csempe gombra kattint, szerezheti be automatikusan Jostle alkalmazás bejelentkezési oldalán.</span><span class="sxs-lookup"><span data-stu-id="2b69b-215">When you click hello Jostle tile in hello Access Panel, you should get automatically login page of Jostle application.</span></span>
<span data-ttu-id="2b69b-216">További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="2b69b-216">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2b69b-217">További források</span><span class="sxs-lookup"><span data-stu-id="2b69b-217">Additional resources</span></span>

* [<span data-ttu-id="2b69b-218">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="2b69b-218">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="2b69b-219">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="2b69b-219">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-jostle-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-jostle-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-jostle-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-jostle-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-jostle-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-jostle-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-jostle-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-jostle-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-jostle-tutorial/tutorial_general_203.png

