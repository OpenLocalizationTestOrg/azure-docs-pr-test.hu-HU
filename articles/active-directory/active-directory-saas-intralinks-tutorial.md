---
title: "Oktatóanyag: Azure Active Directoryval integrált Intralinks |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Intralinks között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 147f2bf9-166b-402e-adc4-4b19dd336883
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: 6fa49c932d0c48d4b48e04fe91af9fc86a0c1cdb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-intralinks"></a><span data-ttu-id="4d932-103">Oktatóanyag: Azure Active Directoryval integrált Intralinks</span><span class="sxs-lookup"><span data-stu-id="4d932-103">Tutorial: Azure Active Directory integration with Intralinks</span></span>

<span data-ttu-id="4d932-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Intralinks az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="4d932-104">In this tutorial, you learn how toointegrate Intralinks with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="4d932-105">Intralinks integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="4d932-105">Integrating Intralinks with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="4d932-106">Megadhatja a hozzáférés tooIntralinks rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="4d932-106">You can control in Azure AD who has access tooIntralinks</span></span>
- <span data-ttu-id="4d932-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooIntralinks (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="4d932-107">You can enable your users tooautomatically get signed-on tooIntralinks (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="4d932-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="4d932-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="4d932-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="4d932-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4d932-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="4d932-110">Prerequisites</span></span>

<span data-ttu-id="4d932-111">az Azure AD integrálása Intralinks tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="4d932-111">tooconfigure Azure AD integration with Intralinks, you need hello following items:</span></span>

- <span data-ttu-id="4d932-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="4d932-112">An Azure AD subscription</span></span>
- <span data-ttu-id="4d932-113">Egy Intralinks egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="4d932-113">An Intralinks single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="4d932-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="4d932-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="4d932-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="4d932-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="4d932-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="4d932-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="4d932-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="4d932-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="4d932-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="4d932-118">Scenario description</span></span>
<span data-ttu-id="4d932-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="4d932-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="4d932-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="4d932-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="4d932-121">Hello gyűjteményből Intralinks hozzáadása</span><span class="sxs-lookup"><span data-stu-id="4d932-121">Adding Intralinks from hello gallery</span></span>
2. <span data-ttu-id="4d932-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="4d932-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-intralinks-from-hello-gallery"></a><span data-ttu-id="4d932-123">Hello gyűjteményből Intralinks hozzáadása</span><span class="sxs-lookup"><span data-stu-id="4d932-123">Adding Intralinks from hello gallery</span></span>
<span data-ttu-id="4d932-124">tooconfigure hello integrációja Intralinks az Azure AD-be, meg kell tooadd Intralinks hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="4d932-124">tooconfigure hello integration of Intralinks into Azure AD, you need tooadd Intralinks from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="4d932-125">**tooadd Intralinks hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="4d932-125">**tooadd Intralinks from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="4d932-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="4d932-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="4d932-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="4d932-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="4d932-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="4d932-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="4d932-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="4d932-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="4d932-133">Hello keresési mezőbe, írja be a **Intralinks**.</span><span class="sxs-lookup"><span data-stu-id="4d932-133">In hello search box, type **Intralinks**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_search.png)

5. <span data-ttu-id="4d932-135">A hello eredmények panelen válassza ki a **Intralinks**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="4d932-135">In hello results panel, select **Intralinks**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="4d932-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="4d932-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="4d932-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Intralinks.</span><span class="sxs-lookup"><span data-stu-id="4d932-138">In this section, you configure and test Azure AD single sign-on with Intralinks based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="4d932-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Intralinks tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="4d932-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Intralinks is tooa user in Azure AD.</span></span> <span data-ttu-id="4d932-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Intralinks közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="4d932-140">In other words, a link relationship between an Azure AD user and hello related user in Intralinks needs toobe established.</span></span>

<span data-ttu-id="4d932-141">Intralinks, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="4d932-141">In Intralinks, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="4d932-142">tooconfigure és az Azure AD az egyszeri bejelentkezés Intralinks-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="4d932-142">tooconfigure and test Azure AD single sign-on with Intralinks, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="4d932-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="4d932-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="4d932-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="4d932-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="4d932-145">**[Egy Intralinks tesztfelhasználó létrehozása](#creating-an-intralinks-test-user)**  -toohave egy megfelelője a Britta Simon a Intralinks, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="4d932-145">**[Creating an Intralinks test user](#creating-an-intralinks-test-user)** - toohave a counterpart of Britta Simon in Intralinks that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="4d932-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="4d932-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="4d932-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="4d932-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="4d932-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="4d932-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="4d932-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Intralinks alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="4d932-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Intralinks application.</span></span>

<span data-ttu-id="4d932-150">**az Azure AD tooconfigure egyszeri bejelentkezést a Intralinks, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="4d932-150">**tooconfigure Azure AD single sign-on with Intralinks, perform hello following steps:**</span></span>

1. <span data-ttu-id="4d932-151">Az Azure portál, a hello hello **Intralinks** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="4d932-151">In hello Azure portal, on hello **Intralinks** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="4d932-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="4d932-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_samlbase.png)

3. <span data-ttu-id="4d932-155">A hello **Intralinks tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="4d932-155">On hello **Intralinks Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_url.png)

    <span data-ttu-id="4d932-157">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<company name>.Intralinks.com/?PartnerIdpId=https://sts.windows.net/<AzureADTenantID>`</span><span class="sxs-lookup"><span data-stu-id="4d932-157">In hello **Sign-on URL** textbox, type a URL using hello following pattern:  `https://<company name>.Intralinks.com/?PartnerIdpId=https://sts.windows.net/<AzureADTenantID>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="4d932-158">Ez az érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="4d932-158">This value is not real.</span></span> <span data-ttu-id="4d932-159">Frissítse ezt az értéket hello tényleges bejelentkezési URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="4d932-159">Update this value with hello actual Sign-On URL.</span></span> <span data-ttu-id="4d932-160">Ügyfél [Intralinks ügyfél-támogatási csoport](https://www.intralinks.com/contact-1) tooget ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="4d932-160">Contact [Intralinks Client support team](https://www.intralinks.com/contact-1) tooget this value.</span></span> 
 
4. <span data-ttu-id="4d932-161">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="4d932-161">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_certificate.png) 

5. <span data-ttu-id="4d932-163">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="4d932-163">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-intralinks-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="4d932-165">tooconfigure egyszeri bejelentkezést a **Intralinks** oldalon kell letöltött toosend hello **metaadatainak XML-kódja** [Intralinks támogatási csoport](https://www.intralinks.com/contact-1).</span><span class="sxs-lookup"><span data-stu-id="4d932-165">tooconfigure single sign-on on **Intralinks** side, you need toosend hello downloaded **Metadata XML** [Intralinks support team](https://www.intralinks.com/contact-1).</span></span> <span data-ttu-id="4d932-166">Maguk állítják be ezt a beállítást toohave hello SAML SSO kapcsolat mindkét oldalán megfelelően beállítva.</span><span class="sxs-lookup"><span data-stu-id="4d932-166">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="4d932-167">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="4d932-167">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="4d932-168">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="4d932-168">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="4d932-169">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="4d932-169">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="4d932-170">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="4d932-170">Creating an Azure AD test user</span></span>
<span data-ttu-id="4d932-171">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="4d932-171">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="4d932-173">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="4d932-173">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="4d932-174">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="4d932-174">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-intralinks-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="4d932-176">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="4d932-176">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-intralinks-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="4d932-178">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="4d932-178">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-intralinks-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="4d932-180">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="4d932-180">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-intralinks-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="4d932-182">a.</span><span class="sxs-lookup"><span data-stu-id="4d932-182">a.</span></span> <span data-ttu-id="4d932-183">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="4d932-183">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="4d932-184">b.</span><span class="sxs-lookup"><span data-stu-id="4d932-184">b.</span></span> <span data-ttu-id="4d932-185">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="4d932-185">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="4d932-186">c.</span><span class="sxs-lookup"><span data-stu-id="4d932-186">c.</span></span> <span data-ttu-id="4d932-187">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="4d932-187">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="4d932-188">d.</span><span class="sxs-lookup"><span data-stu-id="4d932-188">d.</span></span> <span data-ttu-id="4d932-189">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="4d932-189">Click **Create**.</span></span>
 
### <a name="creating-an-intralinks-test-user"></a><span data-ttu-id="4d932-190">Egy Intralinks tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="4d932-190">Creating an Intralinks test user</span></span>

<span data-ttu-id="4d932-191">Ebben a szakaszban egy Intralinks Britta Simon nevű felhasználót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="4d932-191">In this section, you create a user called Britta Simon in Intralinks.</span></span> <span data-ttu-id="4d932-192">Adjon együttműködve [Intralinks támogatási csoport](https://www.intralinks.com/contact-1) tooadd hello felhasználók hello Intralinks platform.</span><span class="sxs-lookup"><span data-stu-id="4d932-192">Please work with [Intralinks support team](https://www.intralinks.com/contact-1) tooadd hello users in hello Intralinks platform.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="4d932-193">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="4d932-193">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="4d932-194">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooIntralinks megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="4d932-194">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooIntralinks.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="4d932-196">**tooassign Britta Simon tooIntralinks, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="4d932-196">**tooassign Britta Simon tooIntralinks, perform hello following steps:**</span></span>

1. <span data-ttu-id="4d932-197">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="4d932-197">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="4d932-199">Hello alkalmazások listában válassza ki a **Intralinks**.</span><span class="sxs-lookup"><span data-stu-id="4d932-199">In hello applications list, select **Intralinks**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_app.png) 

3. <span data-ttu-id="4d932-201">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="4d932-201">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="4d932-203">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="4d932-203">Click **Add** button.</span></span> <span data-ttu-id="4d932-204">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="4d932-204">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="4d932-206">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="4d932-206">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="4d932-207">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="4d932-207">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="4d932-208">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="4d932-208">Click **Assign** button on **Add Assignment** dialog.</span></span>

### <a name="add-intralinks-via-or-elite-application"></a><span data-ttu-id="4d932-209">Intralinks keresztül vagy a Elite alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="4d932-209">Add Intralinks VIA or Elite application</span></span>

<span data-ttu-id="4d932-210">Intralinks által használt összes alkalmazáshoz más Intralinks üzlet Nexus alkalmazás kivételével azonos SSO identitásplatformmal hello.</span><span class="sxs-lookup"><span data-stu-id="4d932-210">Intralinks uses hello same SSO identity platform for all other Intralinks applications excluding Deal Nexus application.</span></span> <span data-ttu-id="4d932-211">Ezért ha azt tervezi, toouse más Intralinks alkalmazás majd először létre kell tooconfigure SSO egy elsődleges Intralinks alkalmazáshoz a fent leírt hello eljárással.</span><span class="sxs-lookup"><span data-stu-id="4d932-211">So if you plan toouse any other Intralinks application then first you have tooconfigure SSO for one Primary Intralinks application using hello procedure described above.</span></span>

<span data-ttu-id="4d932-212">Ezt követően követésével alábbi eljárás tooadd hello Intralinks egy másik alkalmazás az Ön bérlőjében, amely az egyszeri bejelentkezés az elsődleges alkalmazás használhatják fel.</span><span class="sxs-lookup"><span data-stu-id="4d932-212">After that you can follow hello below procedure tooadd another Intralinks application in your tenant which can leverage this primary application for SSO.</span></span> 

>[!NOTE]
><span data-ttu-id="4d932-213">Ez a szolgáltatás nem érhető el egyetlen tooAzure AD Premium Termékváltozat az ügyfelek és az ingyenes vagy alapszintű Termékváltozat ügyfelek esetén nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="4d932-213">This feature is available only tooAzure AD Premium SKU Customers and not available for Free or Basic SKU customers.</span></span>

1. <span data-ttu-id="4d932-214">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="4d932-214">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]


2. <span data-ttu-id="4d932-216">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="4d932-216">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="4d932-217">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="4d932-217">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="4d932-219">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="4d932-219">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="4d932-221">Hello keresési mezőbe, írja be a **Intralinks**.</span><span class="sxs-lookup"><span data-stu-id="4d932-221">In hello search box, type **Intralinks**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_search.png)

5. <span data-ttu-id="4d932-223">A **alkalmazás Intralinks hozzáadása** hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="4d932-223">On **Intralinks Add app** perform hello following steps:</span></span>

    ![Intralinks keresztül vagy a Elite alkalmazás hozzáadása](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_addapp.png)

    <span data-ttu-id="4d932-225">a.</span><span class="sxs-lookup"><span data-stu-id="4d932-225">a.</span></span> <span data-ttu-id="4d932-226">A **neve** szövegmező, adja meg a megfelelő hello alkalmazás neve például **Intralinks Elite**.</span><span class="sxs-lookup"><span data-stu-id="4d932-226">In **Name** textbox, enter appropriate name of hello application e.g. **Intralinks Elite**.</span></span>

    <span data-ttu-id="4d932-227">b.</span><span class="sxs-lookup"><span data-stu-id="4d932-227">b.</span></span> <span data-ttu-id="4d932-228">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="4d932-228">Click **Add** button.</span></span>

6.  <span data-ttu-id="4d932-229">Az Azure portál, a hello hello **Intralinks** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="4d932-229">In hello Azure portal, on hello **Intralinks** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

7. <span data-ttu-id="4d932-231">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **társított bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="4d932-231">On hello **Single sign-on** dialog, select **Mode** as   **Linked Sign-on**.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_linkedsignon.png)

8. <span data-ttu-id="4d932-233">A hello hello SP kezdeményezett egyszeri bejelentkezési URL-cím beszerzése [Intralinks team](https://www.intralinks.com/contact-1) a hello másik Intralinks alkalmazást, és adja meg a **konfigurálása bejelentkezési URL-cím** alább látható módon.</span><span class="sxs-lookup"><span data-stu-id="4d932-233">Get hello hello SP Initiated SSO URL from [Intralinks team](https://www.intralinks.com/contact-1) for hello other Intralinks application and enter it in **Configure Sign-on URL** as shown below.</span></span> 
    
     ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_customappurl.png)
    
     <span data-ttu-id="4d932-235">Hello URL-cím bejelentkezési szövegmezőben írja be a felhasználók toosign tooyour Intralinks alkalmazás használatával a következő mintát hello használt hello URL-cím:</span><span class="sxs-lookup"><span data-stu-id="4d932-235">In hello Sign On URL textbox, type hello URL used by your users toosign-on tooyour Intralinks application using hello following pattern:</span></span>
   
    `https://<company name>.Intralinks.com/?PartnerIdpId=https://sts.windows.net/<AzureADTenantID>`

9. <span data-ttu-id="4d932-236">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="4d932-236">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-intralinks-tutorial/tutorial_general_400.png)

10. <span data-ttu-id="4d932-238">Hello alkalmazás toouser vagy csoport hozzárendelése, lásd a hello szakaszban  **[hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**.</span><span class="sxs-lookup"><span data-stu-id="4d932-238">Assign hello application toouser or groups as shown in hello section **[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)**.</span></span>

### <a name="testing-single-sign-on"></a><span data-ttu-id="4d932-239">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="4d932-239">Testing single sign-on</span></span>

<span data-ttu-id="4d932-240">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="4d932-240">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="4d932-241">Hello Intralinks hello hozzáférési Panel csempére kattintva kapja meg automatikusan bejelentkezett tooyour Intralinks alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="4d932-241">When you click hello Intralinks tile in hello Access Panel, you should get automatically signed-on tooyour Intralinks application.</span></span>
<span data-ttu-id="4d932-242">További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="4d932-242">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4d932-243">További források</span><span class="sxs-lookup"><span data-stu-id="4d932-243">Additional resources</span></span>

* [<span data-ttu-id="4d932-244">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="4d932-244">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="4d932-245">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="4d932-245">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_203.png

