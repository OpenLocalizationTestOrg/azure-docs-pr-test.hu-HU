---
title: "Oktatóanyag: Azure Active Directoryval integrált Cimpl |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Cimpl között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 58ee5481-ae40-4e4a-a3c9-86343851fc9a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2017
ms.author: jeedes
ms.openlocfilehash: 0b993f2b58aaeac24e657060fb31651edc847f98
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-cimpl"></a><span data-ttu-id="15816-103">Oktatóanyag: Azure Active Directoryval integrált Cimpl</span><span class="sxs-lookup"><span data-stu-id="15816-103">Tutorial: Azure Active Directory integration with Cimpl</span></span>

<span data-ttu-id="15816-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Cimpl az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="15816-104">In this tutorial, you learn how toointegrate Cimpl with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="15816-105">Cimpl integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="15816-105">Integrating Cimpl with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="15816-106">Megadhatja a hozzáférés tooCimpl rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="15816-106">You can control in Azure AD who has access tooCimpl</span></span>
- <span data-ttu-id="15816-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooCimpl (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="15816-107">You can enable your users tooautomatically get signed-on tooCimpl (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="15816-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="15816-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="15816-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="15816-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="15816-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="15816-110">Prerequisites</span></span>

<span data-ttu-id="15816-111">az Azure AD integrálása Cimpl tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="15816-111">tooconfigure Azure AD integration with Cimpl, you need hello following items:</span></span>

- <span data-ttu-id="15816-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="15816-112">An Azure AD subscription</span></span>
- <span data-ttu-id="15816-113">Egy Cimpl egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="15816-113">A Cimpl single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="15816-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="15816-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="15816-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="15816-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="15816-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="15816-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="15816-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="15816-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="15816-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="15816-118">Scenario description</span></span>
<span data-ttu-id="15816-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="15816-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="15816-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="15816-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="15816-121">Hello gyűjteményből Cimpl hozzáadása</span><span class="sxs-lookup"><span data-stu-id="15816-121">Adding Cimpl from hello gallery</span></span>
2. <span data-ttu-id="15816-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="15816-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-cimpl-from-hello-gallery"></a><span data-ttu-id="15816-123">Hello gyűjteményből Cimpl hozzáadása</span><span class="sxs-lookup"><span data-stu-id="15816-123">Adding Cimpl from hello gallery</span></span>
<span data-ttu-id="15816-124">tooconfigure hello integrációja Cimpl az Azure AD-be, meg kell tooadd Cimpl hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="15816-124">tooconfigure hello integration of Cimpl into Azure AD, you need tooadd Cimpl from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="15816-125">**tooadd Cimpl hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="15816-125">**tooadd Cimpl from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="15816-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="15816-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="15816-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="15816-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="15816-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="15816-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="15816-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="15816-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="15816-133">Hello keresési mezőbe, írja be a **Cimpl**.</span><span class="sxs-lookup"><span data-stu-id="15816-133">In hello search box, type **Cimpl**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-cimpl-tutorial/tutorial_cimpl_search.png)

5. <span data-ttu-id="15816-135">A hello eredmények panelen válassza ki a **Cimpl**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="15816-135">In hello results panel, select **Cimpl**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-cimpl-tutorial/tutorial_cimpl_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="15816-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="15816-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="15816-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Cimpl.</span><span class="sxs-lookup"><span data-stu-id="15816-138">In this section, you configure and test Azure AD single sign-on with Cimpl based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="15816-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Cimpl tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="15816-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Cimpl is tooa user in Azure AD.</span></span> <span data-ttu-id="15816-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Cimpl közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="15816-140">In other words, a link relationship between an Azure AD user and hello related user in Cimpl needs toobe established.</span></span>

<span data-ttu-id="15816-141">Cimpl, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="15816-141">In Cimpl, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="15816-142">tooconfigure és az Azure AD az egyszeri bejelentkezés Cimpl-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="15816-142">tooconfigure and test Azure AD single sign-on with Cimpl, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="15816-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="15816-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="15816-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="15816-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="15816-145">**[Cimpl tesztfelhasználó létrehozása](#creating-a-cimpl-test-user)**  -toohave egy megfelelője a Britta Simon a Cimpl, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="15816-145">**[Creating a Cimpl test user](#creating-a-cimpl-test-user)** - toohave a counterpart of Britta Simon in Cimpl that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="15816-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="15816-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="15816-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="15816-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="15816-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="15816-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="15816-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Cimpl alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="15816-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Cimpl application.</span></span>

<span data-ttu-id="15816-150">**az Azure AD tooconfigure egyszeri bejelentkezést a Cimpl, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="15816-150">**tooconfigure Azure AD single sign-on with Cimpl, perform hello following steps:**</span></span>

1. <span data-ttu-id="15816-151">Az Azure portál, a hello hello **Cimpl** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="15816-151">In hello Azure portal, on hello **Cimpl** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="15816-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="15816-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cimpl-tutorial/tutorial_cimpl_samlbase.png)

3. <span data-ttu-id="15816-155">A hello **Cimpl tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="15816-155">On hello **Cimpl Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cimpl-tutorial/tutorial_cimpl_url.png)

    <span data-ttu-id="15816-157">a.</span><span class="sxs-lookup"><span data-stu-id="15816-157">a.</span></span> <span data-ttu-id="15816-158">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://sso.etelesolv.com/<TENANTNAME>`</span><span class="sxs-lookup"><span data-stu-id="15816-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://sso.etelesolv.com/<TENANTNAME>`</span></span>

    <span data-ttu-id="15816-159">b.</span><span class="sxs-lookup"><span data-stu-id="15816-159">b.</span></span> <span data-ttu-id="15816-160">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://sso.etelesolv.com/<TENANTNAME>`</span><span class="sxs-lookup"><span data-stu-id="15816-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://sso.etelesolv.com/<TENANTNAME>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="15816-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="15816-161">These values are not real.</span></span> <span data-ttu-id="15816-162">Frissítse a bejelentkezési URL-cím és azonosító a hello tényleges értékek.</span><span class="sxs-lookup"><span data-stu-id="15816-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="15816-163">Lépjen kapcsolatba a Cimpl csapatának **+1 866-982-8250** tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="15816-163">Contact Cimpl team at **+1 866-982-8250** tooget these values.</span></span> 
 
4. <span data-ttu-id="15816-164">A hello **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="15816-164">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cimpl-tutorial/tutorial_cimpl_certificate.png) 

5. <span data-ttu-id="15816-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="15816-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cimpl-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="15816-168">A hello **Cimpl konfigurációs** kattintson **konfigurálása Cimpl** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="15816-168">On hello **Cimpl Configuration** section, click **Configure Cimpl** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="15816-169">Másolás hello **SAML Entitásazonosító és SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="15816-169">Copy hello **SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cimpl-tutorial/tutorial_cimpl_configure.png) 

7. <span data-ttu-id="15816-171">tooconfigure egyszeri bejelentkezést a **Cimpl** oldalon kell letöltött toosend hello **tanúsítvány (Base64)**, **SAML Entitásazonosító és SAML-alapú egyszeri bejelentkezési URL-címe** tooCimpl támogatja a következő **+1 866-982-8250**.</span><span class="sxs-lookup"><span data-stu-id="15816-171">tooconfigure single sign-on on **Cimpl** side, you need toosend hello downloaded **Certificate (Base64)**, **SAML Entity ID, and SAML Single Sign-On Service URL** tooCimpl support at **+1 866-982-8250**.</span></span>


> [!TIP]
> <span data-ttu-id="15816-172">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="15816-172">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="15816-173">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="15816-173">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="15816-174">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="15816-174">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="15816-175">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="15816-175">Creating an Azure AD test user</span></span>
<span data-ttu-id="15816-176">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="15816-176">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="15816-178">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="15816-178">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="15816-179">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="15816-179">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-cimpl-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="15816-181">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="15816-181">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-cimpl-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="15816-183">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="15816-183">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-cimpl-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="15816-185">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="15816-185">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-cimpl-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="15816-187">a.</span><span class="sxs-lookup"><span data-stu-id="15816-187">a.</span></span> <span data-ttu-id="15816-188">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="15816-188">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="15816-189">b.</span><span class="sxs-lookup"><span data-stu-id="15816-189">b.</span></span> <span data-ttu-id="15816-190">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="15816-190">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="15816-191">c.</span><span class="sxs-lookup"><span data-stu-id="15816-191">c.</span></span> <span data-ttu-id="15816-192">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="15816-192">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="15816-193">d.</span><span class="sxs-lookup"><span data-stu-id="15816-193">d.</span></span> <span data-ttu-id="15816-194">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="15816-194">Click **Create**.</span></span>
 
### <a name="creating-a-cimpl-test-user"></a><span data-ttu-id="15816-195">Cimpl tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="15816-195">Creating a Cimpl test user</span></span>

<span data-ttu-id="15816-196">hello ebben a szakaszban célja toocreate Cimpl Britta Simon nevű felhasználó.</span><span class="sxs-lookup"><span data-stu-id="15816-196">hello objective of this section is toocreate a user called Britta Simon in Cimpl.</span></span> <span data-ttu-id="15816-197">Cimpl támogatását együttműködve **+1 866-982-8250** tooadd hello felhasználók hello Cimpl fiók.</span><span class="sxs-lookup"><span data-stu-id="15816-197">Work with Cimpl support at **+1 866-982-8250** tooadd hello users in hello Cimpl account.</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="15816-198">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="15816-198">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="15816-199">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooCimpl megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="15816-199">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooCimpl.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="15816-201">**tooassign Britta Simon tooCimpl, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="15816-201">**tooassign Britta Simon tooCimpl, perform hello following steps:**</span></span>

1. <span data-ttu-id="15816-202">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="15816-202">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="15816-204">Hello alkalmazások listában válassza ki a **Cimpl**.</span><span class="sxs-lookup"><span data-stu-id="15816-204">In hello applications list, select **Cimpl**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cimpl-tutorial/tutorial_cimpl_app.png) 

3. <span data-ttu-id="15816-206">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="15816-206">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="15816-208">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="15816-208">Click **Add** button.</span></span> <span data-ttu-id="15816-209">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="15816-209">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="15816-211">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="15816-211">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="15816-212">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="15816-212">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="15816-213">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="15816-213">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="15816-214">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="15816-214">Testing single sign-on</span></span>

<span data-ttu-id="15816-215">hello ebben a szakaszban célja tootest hozzáférési Panel az Azure AD SSO konfigurációs használatával hello.</span><span class="sxs-lookup"><span data-stu-id="15816-215">hello objective of this section is tootest your Azure AD SSO configuration using hello Access Panel.</span></span>  <span data-ttu-id="15816-216">Ha a hozzáférési Panel hello hello Cimpl csempe gombra kattint, automatikusan bejelentkezett tooyour Cimpl alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="15816-216">When you click hello Cimpl tile in hello Access Panel, you should get automatically signed-on tooyour Cimpl application.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="15816-217">További források</span><span class="sxs-lookup"><span data-stu-id="15816-217">Additional resources</span></span>

* [<span data-ttu-id="15816-218">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="15816-218">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="15816-219">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="15816-219">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-cimpl-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cimpl-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cimpl-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cimpl-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-cimpl-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-cimpl-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-cimpl-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-cimpl-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-cimpl-tutorial/tutorial_general_203.png

