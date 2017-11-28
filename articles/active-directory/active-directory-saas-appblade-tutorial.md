---
title: "Oktatóanyag: Azure Active Directoryval integrált AppBlade |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és AppBlade között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 3360d7aa-6518-4f99-88bd-b7f7258183e8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jeedes
ms.openlocfilehash: 06f3d8fcee97945c867bca6f3aebe15ecef04617
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-appblade"></a><span data-ttu-id="3f3a2-103">Oktatóanyag: Azure Active Directoryval integrált AppBlade</span><span class="sxs-lookup"><span data-stu-id="3f3a2-103">Tutorial: Azure Active Directory integration with AppBlade</span></span>

<span data-ttu-id="3f3a2-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate AppBlade az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="3f3a2-104">In this tutorial, you learn how toointegrate AppBlade with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="3f3a2-105">AppBlade integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="3f3a2-105">Integrating AppBlade with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="3f3a2-106">Megadhatja a hozzáférés tooAppBlade rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="3f3a2-106">You can control in Azure AD who has access tooAppBlade</span></span>
- <span data-ttu-id="3f3a2-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooAppBlade (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="3f3a2-107">You can enable your users tooautomatically get signed-on tooAppBlade (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="3f3a2-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="3f3a2-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="3f3a2-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="3f3a2-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3f3a2-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="3f3a2-110">Prerequisites</span></span>

<span data-ttu-id="3f3a2-111">az Azure AD integrálása AppBlade tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="3f3a2-111">tooconfigure Azure AD integration with AppBlade, you need hello following items:</span></span>

- <span data-ttu-id="3f3a2-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="3f3a2-112">An Azure AD subscription</span></span>
- <span data-ttu-id="3f3a2-113">Egy AppBlade egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="3f3a2-113">An AppBlade single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="3f3a2-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="3f3a2-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="3f3a2-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="3f3a2-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="3f3a2-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="3f3a2-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="3f3a2-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="3f3a2-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="3f3a2-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="3f3a2-118">Scenario description</span></span>
<span data-ttu-id="3f3a2-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="3f3a2-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="3f3a2-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="3f3a2-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="3f3a2-121">Hello gyűjteményből AppBlade hozzáadása</span><span class="sxs-lookup"><span data-stu-id="3f3a2-121">Adding AppBlade from hello gallery</span></span>
2. <span data-ttu-id="3f3a2-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="3f3a2-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-appblade-from-hello-gallery"></a><span data-ttu-id="3f3a2-123">Hello gyűjteményből AppBlade hozzáadása</span><span class="sxs-lookup"><span data-stu-id="3f3a2-123">Adding AppBlade from hello gallery</span></span>
<span data-ttu-id="3f3a2-124">tooconfigure hello integrációja AppBlade az Azure AD-be, meg kell tooadd AppBlade hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="3f3a2-124">tooconfigure hello integration of AppBlade into Azure AD, you need tooadd AppBlade from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="3f3a2-125">**tooadd AppBlade hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="3f3a2-125">**tooadd AppBlade from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="3f3a2-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="3f3a2-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="3f3a2-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="3f3a2-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="3f3a2-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="3f3a2-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="3f3a2-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="3f3a2-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="3f3a2-133">Hello keresési mezőbe, írja be a **AppBlade**.</span><span class="sxs-lookup"><span data-stu-id="3f3a2-133">In hello search box, type **AppBlade**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-appblade-tutorial/tutorial_appblade_search.png)

5. <span data-ttu-id="3f3a2-135">A hello eredmények panelen válassza ki a **AppBlade**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="3f3a2-135">In hello results panel, select **AppBlade**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-appblade-tutorial/tutorial_appblade_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="3f3a2-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="3f3a2-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="3f3a2-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján AppBlade</span><span class="sxs-lookup"><span data-stu-id="3f3a2-138">In this section, you configure and test Azure AD single sign-on with AppBlade based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="3f3a2-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó AppBlade tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="3f3a2-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in AppBlade is tooa user in Azure AD.</span></span> <span data-ttu-id="3f3a2-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello AppBlade közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="3f3a2-140">In other words, a link relationship between an Azure AD user and hello related user in AppBlade needs toobe established.</span></span>

<span data-ttu-id="3f3a2-141">AppBlade, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="3f3a2-141">In AppBlade, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="3f3a2-142">tooconfigure és az Azure AD az egyszeri bejelentkezés AppBlade-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="3f3a2-142">tooconfigure and test Azure AD single sign-on with AppBlade, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="3f3a2-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="3f3a2-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="3f3a2-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="3f3a2-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="3f3a2-145">**[Egy AppBlade tesztfelhasználó létrehozása](#creating-an-appblade-test-user)**  -toohave egy megfelelője a Britta Simon a AppBlade, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="3f3a2-145">**[Creating an AppBlade test user](#creating-an-appblade-test-user)** - toohave a counterpart of Britta Simon in AppBlade that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="3f3a2-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="3f3a2-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="3f3a2-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="3f3a2-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="3f3a2-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="3f3a2-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="3f3a2-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az AppBlade alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="3f3a2-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your AppBlade application.</span></span>

<span data-ttu-id="3f3a2-150">**az Azure AD tooconfigure egyszeri bejelentkezést a AppBlade, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="3f3a2-150">**tooconfigure Azure AD single sign-on with AppBlade, perform hello following steps:**</span></span>

1. <span data-ttu-id="3f3a2-151">Az Azure portál, a hello hello **AppBlade** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="3f3a2-151">In hello Azure portal, on hello **AppBlade** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="3f3a2-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="3f3a2-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-appblade-tutorial/tutorial_appblade_samlbase.png)

3. <span data-ttu-id="3f3a2-155">A hello **AppBlade tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="3f3a2-155">On hello **AppBlade Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-appblade-tutorial/tutorial_appblade_url.png)

    <span data-ttu-id="3f3a2-157">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<companyname>.appblade.com/saml/<tenantid>`</span><span class="sxs-lookup"><span data-stu-id="3f3a2-157">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<companyname>.appblade.com/saml/<tenantid>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="3f3a2-158">Ez az érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="3f3a2-158">This value is not real.</span></span> <span data-ttu-id="3f3a2-159">Frissítés hello értékének hello tényleges bejelentkezési URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="3f3a2-159">Update hello value with hello actual Sign-On URL.</span></span> <span data-ttu-id="3f3a2-160">Ügyfél [AppBlade ügyfél-támogatási csoport](mailto:support@appblade.com) tooget hello érték.</span><span class="sxs-lookup"><span data-stu-id="3f3a2-160">Contact [AppBlade Client support team](mailto:support@appblade.com) tooget hello value.</span></span> 
 
4. <span data-ttu-id="3f3a2-161">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="3f3a2-161">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-appblade-tutorial/tutorial_appblade_certificate.png) 

5. <span data-ttu-id="3f3a2-163">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="3f3a2-163">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-appblade-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="3f3a2-165">tooconfigure egyszeri bejelentkezést a **AppBlade** oldalon kell letöltött toosend hello **metaadatainak XML-kódja** túl[AppBlade támogatási csoport](mailto:support@appblade.com).</span><span class="sxs-lookup"><span data-stu-id="3f3a2-165">tooconfigure single sign-on on **AppBlade** side, you need toosend hello downloaded **Metadata XML** too[AppBlade support team](mailto:support@appblade.com).</span></span> <span data-ttu-id="3f3a2-166">Is, kérje meg őket tooconfigure hello **SSO kiállítójának URL-címe** , `https://appblade.com/saml`.</span><span class="sxs-lookup"><span data-stu-id="3f3a2-166">Also, please ask them tooconfigure hello **SSO Issuer URL** as `https://appblade.com/saml`.</span></span> <span data-ttu-id="3f3a2-167">Ez a beállítás egyszeri bejelentkezés toowork szükség.</span><span class="sxs-lookup"><span data-stu-id="3f3a2-167">This setting is required for single sign-on toowork.</span></span>


> [!TIP]
> <span data-ttu-id="3f3a2-168">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="3f3a2-168">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="3f3a2-169">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="3f3a2-169">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="3f3a2-170">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="3f3a2-170">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
 
### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="3f3a2-171">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="3f3a2-171">Creating an Azure AD test user</span></span>
<span data-ttu-id="3f3a2-172">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="3f3a2-172">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="3f3a2-174">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="3f3a2-174">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="3f3a2-175">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="3f3a2-175">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-appblade-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="3f3a2-177">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="3f3a2-177">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-appblade-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="3f3a2-179">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="3f3a2-179">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-appblade-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="3f3a2-181">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="3f3a2-181">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-appblade-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="3f3a2-183">a.</span><span class="sxs-lookup"><span data-stu-id="3f3a2-183">a.</span></span> <span data-ttu-id="3f3a2-184">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="3f3a2-184">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="3f3a2-185">b.</span><span class="sxs-lookup"><span data-stu-id="3f3a2-185">b.</span></span> <span data-ttu-id="3f3a2-186">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="3f3a2-186">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="3f3a2-187">c.</span><span class="sxs-lookup"><span data-stu-id="3f3a2-187">c.</span></span> <span data-ttu-id="3f3a2-188">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="3f3a2-188">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="3f3a2-189">d.</span><span class="sxs-lookup"><span data-stu-id="3f3a2-189">d.</span></span> <span data-ttu-id="3f3a2-190">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="3f3a2-190">Click **Create**.</span></span>
 
### <a name="creating-an-appblade-test-user"></a><span data-ttu-id="3f3a2-191">Egy AppBlade tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="3f3a2-191">Creating an AppBlade test user</span></span>

<span data-ttu-id="3f3a2-192">hello ebben a szakaszban célja toocreate AppBlade Britta Simon nevű felhasználó.</span><span class="sxs-lookup"><span data-stu-id="3f3a2-192">hello objective of this section is toocreate a user called Britta Simon in AppBlade.</span></span> <span data-ttu-id="3f3a2-193">AppBlade támogatja just-in-time kiosztást, amely alapértelmezés szerint van engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="3f3a2-193">AppBlade supports just-in-time provisioning, which is by default enabled.</span></span> <span data-ttu-id="3f3a2-194">**Győződjön meg arról, hogy a tartománynév beállítása a AppBlade a felhasználók átadása. Követően, hogy csak hello just-in-time felhasználók átadásához működik.**</span><span class="sxs-lookup"><span data-stu-id="3f3a2-194">**Make sure that your domain name is configured with AppBlade for user provisioning. After that only hello just-in-time user provisioning works.**</span></span>

<span data-ttu-id="3f3a2-195">Ha hello felhasználói végződő hello tartomány AppBlade állította be a fiókot, majd hello felhasználói számítógépei automatikusan csatlakoznak a hello fiók tagja hello jogosultsági szint megadja az e-mail címmel rendelkezik, amelyek egyike a "Basic" (alapszintű felhasználó csak telepíthető alkalmazások), "Csapattag" (olyan felhasználó, aki projektek kezelését és feltöltését alkalmazás új verziója is), vagy a "Rendszergazda" (teljes körű rendszergazdai jogosultságokkal toohello fiókot).</span><span class="sxs-lookup"><span data-stu-id="3f3a2-195">If hello user has an email address ending with hello domain configured by AppBlade for your account, then hello user will automatically join hello account as a member with hello permission level you specify, which is one of "Basic" (a basic user who can only install applications), "Team Member" (a user who can upload new app versions and manage projects), or "Administrator" (full admin privileges toohello account).</span></span> <span data-ttu-id="3f3a2-196">Általában egy ehhez válassza ki a Basic és előreléphet a felhasználókat manuálisan egy rendszergazdai bejelentkezés (AppBlade előre kell tooconfigure vagy egy e-mail-alapú rendszergazdai bejelentkezés vagy lépteti elő a felhasználó bejelentkezése után hello ügyfél nevében).</span><span class="sxs-lookup"><span data-stu-id="3f3a2-196">Normally one would choose Basic and then promote users manually via an Admin login (AppBlade needs tooconfigure either an email-based admin login in advance or promote a user on behalf of hello customer after login).</span></span>

<span data-ttu-id="3f3a2-197">Nincs ebben a szakaszban az Ön művelet elem.</span><span class="sxs-lookup"><span data-stu-id="3f3a2-197">There is no action item for you in this section.</span></span> <span data-ttu-id="3f3a2-198">Új felhasználó jön létre egy kísérlet tooaccess AppBlade során, ha még nem létezik.</span><span class="sxs-lookup"><span data-stu-id="3f3a2-198">A new user is created during an attempt tooaccess AppBlade if it doesn't exist yet.</span></span> 

> [!NOTE]
> <span data-ttu-id="3f3a2-199">A felhasználó toocreate manuálisan kell, ha szüksége van-e toocontact hello [AppBlade támogatási csoport](mailto:support@appblade.com).</span><span class="sxs-lookup"><span data-stu-id="3f3a2-199">If you need toocreate a user manually, you need toocontact hello [AppBlade support team](mailto:support@appblade.com).</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="3f3a2-200">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="3f3a2-200">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="3f3a2-201">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooAppBlade megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="3f3a2-201">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooAppBlade.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="3f3a2-203">**tooassign Britta Simon tooAppBlade, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="3f3a2-203">**tooassign Britta Simon tooAppBlade, perform hello following steps:**</span></span>

1. <span data-ttu-id="3f3a2-204">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="3f3a2-204">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="3f3a2-206">Hello alkalmazások listában válassza ki a **AppBlade**.</span><span class="sxs-lookup"><span data-stu-id="3f3a2-206">In hello applications list, select **AppBlade**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-appblade-tutorial/tutorial_appblade_app.png) 

3. <span data-ttu-id="3f3a2-208">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="3f3a2-208">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="3f3a2-210">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="3f3a2-210">Click **Add** button.</span></span> <span data-ttu-id="3f3a2-211">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="3f3a2-211">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="3f3a2-213">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="3f3a2-213">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="3f3a2-214">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="3f3a2-214">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="3f3a2-215">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="3f3a2-215">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="3f3a2-216">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="3f3a2-216">Testing single sign-on</span></span>

<span data-ttu-id="3f3a2-217">hello ebben a szakaszban célja tootest az egyszeri bejelentkezés konfigurációs használatával hello a hozzáférési Panel.</span><span class="sxs-lookup"><span data-stu-id="3f3a2-217">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>  
<span data-ttu-id="3f3a2-218">Ha a hozzáférési Panel hello hello AppBlade csempe gombra kattint, automatikusan bejelentkezett tooyour AppBlade alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="3f3a2-218">When you click hello AppBlade tile in hello Access Panel, you should get automatically signed-on tooyour AppBlade application.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="3f3a2-219">További források</span><span class="sxs-lookup"><span data-stu-id="3f3a2-219">Additional resources</span></span>

* [<span data-ttu-id="3f3a2-220">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="3f3a2-220">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="3f3a2-221">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="3f3a2-221">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-appblade-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-appblade-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-appblade-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-appblade-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-appblade-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-appblade-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-appblade-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-appblade-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-appblade-tutorial/tutorial_general_203.png

