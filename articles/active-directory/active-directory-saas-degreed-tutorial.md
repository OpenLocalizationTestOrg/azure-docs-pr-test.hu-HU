---
title: "Oktatóanyag: Azure Active Directoryval integrált Degreed |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Degreed között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 1eda2d1c-b5e2-4c53-ad46-bbeb91cd119a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/10/2017
ms.author: jeedes
ms.openlocfilehash: decb553a6c5fa253ddf16b0f03336ab9366a8b4e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-degreed"></a><span data-ttu-id="944ea-103">Oktatóanyag: Azure Active Directoryval integrált Degreed</span><span class="sxs-lookup"><span data-stu-id="944ea-103">Tutorial: Azure Active Directory integration with Degreed</span></span>

<span data-ttu-id="944ea-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Degreed az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="944ea-104">In this tutorial, you learn how toointegrate Degreed with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="944ea-105">Degreed integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="944ea-105">Integrating Degreed with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="944ea-106">Megadhatja a hozzáférés tooDegreed rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="944ea-106">You can control in Azure AD who has access tooDegreed</span></span>
- <span data-ttu-id="944ea-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooDegreed (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="944ea-107">You can enable your users tooautomatically get signed-on tooDegreed (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="944ea-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="944ea-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="944ea-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="944ea-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="944ea-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="944ea-110">Prerequisites</span></span>

<span data-ttu-id="944ea-111">az Azure AD integrálása Degreed tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="944ea-111">tooconfigure Azure AD integration with Degreed, you need hello following items:</span></span>

- <span data-ttu-id="944ea-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="944ea-112">An Azure AD subscription</span></span>
- <span data-ttu-id="944ea-113">Egy Degreed egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="944ea-113">A Degreed single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="944ea-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="944ea-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="944ea-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="944ea-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="944ea-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="944ea-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="944ea-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="944ea-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="944ea-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="944ea-118">Scenario description</span></span>
<span data-ttu-id="944ea-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="944ea-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="944ea-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="944ea-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="944ea-121">Hello gyűjteményből Degreed hozzáadása</span><span class="sxs-lookup"><span data-stu-id="944ea-121">Adding Degreed from hello gallery</span></span>
2. <span data-ttu-id="944ea-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="944ea-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-degreed-from-hello-gallery"></a><span data-ttu-id="944ea-123">Hello gyűjteményből Degreed hozzáadása</span><span class="sxs-lookup"><span data-stu-id="944ea-123">Adding Degreed from hello gallery</span></span>
<span data-ttu-id="944ea-124">tooconfigure hello integrációja Degreed az Azure AD-be, meg kell tooadd Degreed hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="944ea-124">tooconfigure hello integration of Degreed into Azure AD, you need tooadd Degreed from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="944ea-125">**tooadd Degreed hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="944ea-125">**tooadd Degreed from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="944ea-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="944ea-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="944ea-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="944ea-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="944ea-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="944ea-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="944ea-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="944ea-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="944ea-133">Hello keresési mezőbe, írja be a **Degreed**.</span><span class="sxs-lookup"><span data-stu-id="944ea-133">In hello search box, type **Degreed**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-degreed-tutorial/tutorial_degreed_search.png)

5. <span data-ttu-id="944ea-135">A hello eredmények panelen válassza ki a **Degreed**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="944ea-135">In hello results panel, select **Degreed**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-degreed-tutorial/tutorial_degreed_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="944ea-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="944ea-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="944ea-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján Degreed</span><span class="sxs-lookup"><span data-stu-id="944ea-138">In this section, you configure and test Azure AD single sign-on with Degreed based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="944ea-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Degreed tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="944ea-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Degreed is tooa user in Azure AD.</span></span> <span data-ttu-id="944ea-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Degreed közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="944ea-140">In other words, a link relationship between an Azure AD user and hello related user in Degreed needs toobe established.</span></span>

<span data-ttu-id="944ea-141">Degreed, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="944ea-141">In Degreed, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="944ea-142">tooconfigure és az Azure AD az egyszeri bejelentkezés Degreed-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="944ea-142">tooconfigure and test Azure AD single sign-on with Degreed, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="944ea-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="944ea-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="944ea-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="944ea-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="944ea-145">**[Degreed tesztfelhasználó létrehozása](#creating-a-degreed-test-user)**  -toohave egy megfelelője a Britta Simon a Degreed, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="944ea-145">**[Creating a Degreed test user](#creating-a-degreed-test-user)** - toohave a counterpart of Britta Simon in Degreed that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="944ea-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="944ea-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="944ea-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="944ea-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="944ea-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="944ea-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="944ea-149">Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése, és az Degreed alkalmazásban egyszeri bejelentkezés beállítása.</span><span class="sxs-lookup"><span data-stu-id="944ea-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Degreed application.</span></span>

<span data-ttu-id="944ea-150">**az Azure AD tooconfigure egyszeri bejelentkezést a Degreed, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="944ea-150">**tooconfigure Azure AD single sign-on with Degreed, perform hello following steps:**</span></span>

1. <span data-ttu-id="944ea-151">Az Azure portál, a hello hello **Degreed** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="944ea-151">In hello Azure portal, on hello **Degreed** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="944ea-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="944ea-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-degreed-tutorial/tutorial_degreed_samlbase.png)

3. <span data-ttu-id="944ea-155">A hello **Degreed tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="944ea-155">On hello **Degreed Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-degreed-tutorial/tutorial_degreed_url.png)

    <span data-ttu-id="944ea-157">a.</span><span class="sxs-lookup"><span data-stu-id="944ea-157">a.</span></span> <span data-ttu-id="944ea-158">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://degreed.com/?orgsso=<company code>`</span><span class="sxs-lookup"><span data-stu-id="944ea-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://degreed.com/?orgsso=<company code>`</span></span>

    <span data-ttu-id="944ea-159">b.</span><span class="sxs-lookup"><span data-stu-id="944ea-159">b.</span></span> <span data-ttu-id="944ea-160">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://degreed.com/<instancename>`</span><span class="sxs-lookup"><span data-stu-id="944ea-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://degreed.com/<instancename>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="944ea-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="944ea-161">These values are not real.</span></span> <span data-ttu-id="944ea-162">Frissítse a bejelentkezési URL-cím és azonosító a hello tényleges értékek.</span><span class="sxs-lookup"><span data-stu-id="944ea-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="944ea-163">Ügyfél [Degreed ügyfél-támogatási csoport](mailTo:admin@degreed.com) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="944ea-163">Contact [Degreed Client support team](mailTo:admin@degreed.com) tooget these values.</span></span> 
 


4. <span data-ttu-id="944ea-164">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="944ea-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-degreed-tutorial/tutorial_degreed_certificate.png) 

5. <span data-ttu-id="944ea-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="944ea-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-degreed-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="944ea-168">tooconfigure egyszeri bejelentkezést a **Degreed** oldalon kell letöltött toosend hello **metaadatainak XML-kódja** túl[Degreed támogatási csoport](mailTo:admin@degreed.com).</span><span class="sxs-lookup"><span data-stu-id="944ea-168">tooconfigure single sign-on on **Degreed** side, you need toosend hello downloaded **Metadata XML** too[Degreed support team](mailTo:admin@degreed.com).</span></span> <span data-ttu-id="944ea-169">Maguk állítják be ezt a beállítást toohave hello SAML SSO kapcsolat mindkét oldalán megfelelően beállítva.</span><span class="sxs-lookup"><span data-stu-id="944ea-169">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="944ea-170">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="944ea-170">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="944ea-171">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="944ea-171">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="944ea-172">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="944ea-172">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="944ea-173">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="944ea-173">Creating an Azure AD test user</span></span>
<span data-ttu-id="944ea-174">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="944ea-174">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="944ea-176">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="944ea-176">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="944ea-177">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="944ea-177">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-degreed-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="944ea-179">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="944ea-179">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-degreed-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="944ea-181">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="944ea-181">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-degreed-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="944ea-183">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="944ea-183">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-degreed-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="944ea-185">a.</span><span class="sxs-lookup"><span data-stu-id="944ea-185">a.</span></span> <span data-ttu-id="944ea-186">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="944ea-186">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="944ea-187">b.</span><span class="sxs-lookup"><span data-stu-id="944ea-187">b.</span></span> <span data-ttu-id="944ea-188">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="944ea-188">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="944ea-189">c.</span><span class="sxs-lookup"><span data-stu-id="944ea-189">c.</span></span> <span data-ttu-id="944ea-190">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="944ea-190">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="944ea-191">d.</span><span class="sxs-lookup"><span data-stu-id="944ea-191">d.</span></span> <span data-ttu-id="944ea-192">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="944ea-192">Click **Create**.</span></span>
 
### <a name="creating-a-degreed-test-user"></a><span data-ttu-id="944ea-193">Degreed tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="944ea-193">Creating a Degreed test user</span></span>

<span data-ttu-id="944ea-194">hello ebben a szakaszban célja toocreate Degreed Britta Simon nevű felhasználó.</span><span class="sxs-lookup"><span data-stu-id="944ea-194">hello objective of this section is toocreate a user called Britta Simon in Degreed.</span></span> <span data-ttu-id="944ea-195">Degreed támogatja just-in-time kiosztást, amely alapértelmezés szerint van engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="944ea-195">Degreed supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="944ea-196">Nincs ebben a szakaszban az Ön művelet elem.</span><span class="sxs-lookup"><span data-stu-id="944ea-196">There is no action item for you in this section.</span></span> <span data-ttu-id="944ea-197">Új felhasználó jön létre egy kísérlet tooaccess Degreed során, ha még nem létezik.</span><span class="sxs-lookup"><span data-stu-id="944ea-197">A new user is created during an attempt tooaccess Degreed if it doesn't exist yet.</span></span>

> [!NOTE]
> <span data-ttu-id="944ea-198">A felhasználó toocreate manuálisan kell, ha szüksége van-e toocontact hello [Degreed támogatási csoport](mailTo:admin@degreed.com).</span><span class="sxs-lookup"><span data-stu-id="944ea-198">If you need toocreate a user manually, you need toocontact hello [Degreed support team](mailTo:admin@degreed.com).</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="944ea-199">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="944ea-199">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="944ea-200">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooDegreed megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="944ea-200">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooDegreed.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="944ea-202">**tooassign Britta Simon tooDegreed, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="944ea-202">**tooassign Britta Simon tooDegreed, perform hello following steps:**</span></span>

1. <span data-ttu-id="944ea-203">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="944ea-203">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="944ea-205">Hello alkalmazások listában válassza ki a **Degreed**.</span><span class="sxs-lookup"><span data-stu-id="944ea-205">In hello applications list, select **Degreed**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-degreed-tutorial/tutorial_degreed_app.png) 

3. <span data-ttu-id="944ea-207">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="944ea-207">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="944ea-209">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="944ea-209">Click **Add** button.</span></span> <span data-ttu-id="944ea-210">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="944ea-210">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="944ea-212">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="944ea-212">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="944ea-213">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="944ea-213">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="944ea-214">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="944ea-214">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="944ea-215">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="944ea-215">Testing single sign-on</span></span>

<span data-ttu-id="944ea-216">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="944ea-216">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="944ea-217">Hello Degreed csempére kattintva a hozzáférési Panel hello, automatikusan bejelentkezett tooyour Degreed alkalmazás kapja meg.</span><span class="sxs-lookup"><span data-stu-id="944ea-217">When you click hello Degreed tile in hello Access Panel, you should get automatically signed-on tooyour Degreed application.</span></span>
<span data-ttu-id="944ea-218">További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="944ea-218">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="944ea-219">További források</span><span class="sxs-lookup"><span data-stu-id="944ea-219">Additional resources</span></span>

* [<span data-ttu-id="944ea-220">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="944ea-220">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="944ea-221">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="944ea-221">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-degreed-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-degreed-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-degreed-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-degreed-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-degreed-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-degreed-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-degreed-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-degreed-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-degreed-tutorial/tutorial_general_203.png

