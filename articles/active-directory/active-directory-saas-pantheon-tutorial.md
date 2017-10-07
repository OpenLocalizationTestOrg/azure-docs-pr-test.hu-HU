---
title: "Oktatóanyag: Azure Active Directoryval integrált Pantheon |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Pantheon között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: d2c965d1-666f-44c2-b08f-b73163096374
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes
ms.openlocfilehash: 5c3e54aef1f64dbab77d40a8c098172d609bfec8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-pantheon"></a><span data-ttu-id="37159-103">Oktatóanyag: Azure Active Directoryval integrált Pantheon</span><span class="sxs-lookup"><span data-stu-id="37159-103">Tutorial: Azure Active Directory integration with Pantheon</span></span>

<span data-ttu-id="37159-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Pantheon az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="37159-104">In this tutorial, you learn how toointegrate Pantheon with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="37159-105">Pantheon integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="37159-105">Integrating Pantheon with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="37159-106">Megadhatja a hozzáférés tooPantheon rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="37159-106">You can control in Azure AD who has access tooPantheon</span></span>
- <span data-ttu-id="37159-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooPantheon (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="37159-107">You can enable your users tooautomatically get signed-on tooPantheon (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="37159-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="37159-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="37159-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="37159-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="37159-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="37159-110">Prerequisites</span></span>

<span data-ttu-id="37159-111">az Azure AD integrálása Pantheon tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="37159-111">tooconfigure Azure AD integration with Pantheon, you need hello following items:</span></span>

- <span data-ttu-id="37159-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="37159-112">An Azure AD subscription</span></span>
- <span data-ttu-id="37159-113">Egy Pantheon egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="37159-113">A Pantheon single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="37159-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="37159-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="37159-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="37159-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="37159-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="37159-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="37159-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="37159-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="37159-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="37159-118">Scenario description</span></span>
<span data-ttu-id="37159-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="37159-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="37159-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="37159-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="37159-121">Hello gyűjteményből Pantheon hozzáadása</span><span class="sxs-lookup"><span data-stu-id="37159-121">Adding Pantheon from hello gallery</span></span>
2. <span data-ttu-id="37159-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="37159-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-pantheon-from-hello-gallery"></a><span data-ttu-id="37159-123">Hello gyűjteményből Pantheon hozzáadása</span><span class="sxs-lookup"><span data-stu-id="37159-123">Adding Pantheon from hello gallery</span></span>
<span data-ttu-id="37159-124">tooconfigure hello integrációja Pantheon az Azure AD-be, meg kell tooadd Pantheon hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="37159-124">tooconfigure hello integration of Pantheon into Azure AD, you need tooadd Pantheon from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="37159-125">**tooadd Pantheon hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="37159-125">**tooadd Pantheon from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="37159-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="37159-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="37159-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="37159-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="37159-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="37159-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="37159-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="37159-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="37159-133">Hello keresési mezőbe, írja be a **Pantheon**.</span><span class="sxs-lookup"><span data-stu-id="37159-133">In hello search box, type **Pantheon**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-pantheon-tutorial/tutorial_pantheon_search.png)

5. <span data-ttu-id="37159-135">A hello eredmények panelen válassza ki a **Pantheon**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="37159-135">In hello results panel, select **Pantheon**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-pantheon-tutorial/tutorial_pantheon_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="37159-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="37159-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="37159-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Pantheon.</span><span class="sxs-lookup"><span data-stu-id="37159-138">In this section, you configure and test Azure AD single sign-on with Pantheon based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="37159-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Pantheon tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="37159-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Pantheon is tooa user in Azure AD.</span></span> <span data-ttu-id="37159-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Pantheon közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="37159-140">In other words, a link relationship between an Azure AD user and hello related user in Pantheon needs toobe established.</span></span>

<span data-ttu-id="37159-141">Pantheon, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="37159-141">In Pantheon, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="37159-142">tooconfigure és az Azure AD az egyszeri bejelentkezés Pantheon-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="37159-142">tooconfigure and test Azure AD single sign-on with Pantheon, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="37159-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="37159-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="37159-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="37159-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="37159-145">**[Pantheon tesztfelhasználó létrehozása](#creating-a-pantheon-test-user)**  -toohave egy megfelelője a Britta Simon a Pantheon, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="37159-145">**[Creating a Pantheon test user](#creating-a-pantheon-test-user)** - toohave a counterpart of Britta Simon in Pantheon that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="37159-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="37159-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="37159-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="37159-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="37159-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="37159-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="37159-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Pantheon alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="37159-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Pantheon application.</span></span>

<span data-ttu-id="37159-150">**az Azure AD tooconfigure egyszeri bejelentkezést a Pantheon, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="37159-150">**tooconfigure Azure AD single sign-on with Pantheon, perform hello following steps:**</span></span>

1. <span data-ttu-id="37159-151">Az Azure portál, a hello hello **Pantheon** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="37159-151">In hello Azure portal, on hello **Pantheon** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="37159-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="37159-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-pantheon-tutorial/tutorial_pantheon_samlbase.png)

3. <span data-ttu-id="37159-155">A hello **Pantheon tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="37159-155">On hello **Pantheon Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-pantheon-tutorial/tutorial_pantheon_url.png)

    <span data-ttu-id="37159-157">a.</span><span class="sxs-lookup"><span data-stu-id="37159-157">a.</span></span> <span data-ttu-id="37159-158">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`urn:auth0:pantheon:<orgname>-SSO`</span><span class="sxs-lookup"><span data-stu-id="37159-158">In hello **Identifier** textbox, type a URL using hello following pattern: `urn:auth0:pantheon:<orgname>-SSO`</span></span>

    <span data-ttu-id="37159-159">b.</span><span class="sxs-lookup"><span data-stu-id="37159-159">b.</span></span> <span data-ttu-id="37159-160">A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://pantheon.auth0.com/login/callback?connection=<orgname>-SSO`</span><span class="sxs-lookup"><span data-stu-id="37159-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://pantheon.auth0.com/login/callback?connection=<orgname>-SSO`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="37159-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="37159-161">These values are not real.</span></span> <span data-ttu-id="37159-162">Frissítheti ezeket az értékeket hello tényleges azonosítója és a válasz URL-címmel.</span><span class="sxs-lookup"><span data-stu-id="37159-162">Update these values with hello actual Identifier and Reply URL.</span></span> <span data-ttu-id="37159-163">Ügyfél [Pantheon támogatási csoport](https://pantheon.io/docs/getting-support/) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="37159-163">Contact [Pantheon support team](https://pantheon.io/docs/getting-support/) tooget these values.</span></span>

4. <span data-ttu-id="37159-164">Pantheon alkalmazás adott formátum, ami akkor tooset hello UserIdentifier attribútumérték hello felhasználói e-mail címmel kell hello SAML-előfeltétel vár.</span><span class="sxs-lookup"><span data-stu-id="37159-164">Pantheon application expects hello SAML assertion in specific format, which requires you tooset hello UserIdentifier attribute value with hello user’s email address.</span></span> <span data-ttu-id="37159-165">Alapértelmezés szerint az Azure AD UserIdentifier attribútum hello UserPrincipalName használja.</span><span class="sxs-lookup"><span data-stu-id="37159-165">By default Azure AD uses hello UserPrincipalName for UserIdentifier attribute.</span></span> <span data-ttu-id="37159-166">De sikeres integrációs tooadjust ezen érték toomatch a felhasználó e-mail címét.</span><span class="sxs-lookup"><span data-stu-id="37159-166">But for successful integration you need tooadjust this value toomatch with user’s email address.</span></span> <span data-ttu-id="37159-167">hello integráció csak ezután hello helyes leképezés fog működni.</span><span class="sxs-lookup"><span data-stu-id="37159-167">hello integration will only work after doing hello correct mapping.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-pantheon-tutorial/tutorial_attribute.png)  


5. <span data-ttu-id="37159-169">A hello **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="37159-169">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-pantheon-tutorial/tutorial_pantheon_certificate.png)

6. <span data-ttu-id="37159-171">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="37159-171">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-pantheon-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="37159-173">A hello **Pantheon konfigurációs** kattintson **konfigurálása Pantheon** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="37159-173">On hello **Pantheon Configuration** section, click **Configure Pantheon** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="37159-174">Másolás hello **SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="37159-174">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-pantheon-tutorial/tutorial_pantheon_configure.png) 

8. <span data-ttu-id="37159-176">tooconfigure egyszeri bejelentkezést a **Pantheon** oldalon kell letöltött toosend hello **tanúsítvány** és **SAML-alapú egyszeri bejelentkezési URL-címe** túl[Pantheon támogatási csoport](https://pantheon.io/docs/getting-support/).</span><span class="sxs-lookup"><span data-stu-id="37159-176">tooconfigure single sign-on on **Pantheon** side, you need toosend hello downloaded **Certificate** and **SAML Single Sign-On Service URL** too[Pantheon support team](https://pantheon.io/docs/getting-support/).</span></span>

     > [!Note]
     > <span data-ttu-id="37159-177">Szükség tooprovide hello E-mail tartomány(ok) információkat és a dátum idő Ha azt szeretné, tooenable ezt a kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="37159-177">You also need tooprovide hello Email Domain(s) information and Date Time when you want tooenable this connection.</span></span> <span data-ttu-id="37159-178">További információt az található [Itt](https://pantheon.io/docs/sso-organizations/)</span><span class="sxs-lookup"><span data-stu-id="37159-178">You can find more details about it from [here](https://pantheon.io/docs/sso-organizations/)</span></span>

> [!TIP]
> <span data-ttu-id="37159-179">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="37159-179">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="37159-180">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="37159-180">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="37159-181">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="37159-181">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="37159-182">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="37159-182">Creating an Azure AD test user</span></span>
<span data-ttu-id="37159-183">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="37159-183">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="37159-185">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="37159-185">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="37159-186">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="37159-186">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-pantheon-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="37159-188">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="37159-188">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-pantheon-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="37159-190">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="37159-190">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-pantheon-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="37159-192">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="37159-192">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-pantheon-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="37159-194">a.</span><span class="sxs-lookup"><span data-stu-id="37159-194">a.</span></span> <span data-ttu-id="37159-195">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="37159-195">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="37159-196">b.</span><span class="sxs-lookup"><span data-stu-id="37159-196">b.</span></span> <span data-ttu-id="37159-197">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="37159-197">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="37159-198">c.</span><span class="sxs-lookup"><span data-stu-id="37159-198">c.</span></span> <span data-ttu-id="37159-199">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="37159-199">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="37159-200">d.</span><span class="sxs-lookup"><span data-stu-id="37159-200">d.</span></span> <span data-ttu-id="37159-201">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="37159-201">Click **Create**.</span></span>
 
### <a name="creating-a-pantheon-test-user"></a><span data-ttu-id="37159-202">Pantheon tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="37159-202">Creating a Pantheon test user</span></span>

<span data-ttu-id="37159-203">Ebben a szakaszban egy Pantheon Britta Simon nevű felhasználót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="37159-203">In this section, you create a user called Britta Simon in Pantheon.</span></span> <span data-ttu-id="37159-204">Kövesse az alábbi lépéseket tooadd hello felhasználó Pantheon hello.</span><span class="sxs-lookup"><span data-stu-id="37159-204">Please follow hello below steps tooadd hello user in Pantheon.</span></span> 

>[!NOTE] 
><span data-ttu-id="37159-205">Az egyszeri bejelentkezés toowork felhasználói toobe Pantheon először létre kell.</span><span class="sxs-lookup"><span data-stu-id="37159-205">For SSO toowork user needs toobe created first in Pantheon.</span></span>

1. <span data-ttu-id="37159-206">Bejelentkezési tooPantheon rendszergazdai hitelesítő adataival.</span><span class="sxs-lookup"><span data-stu-id="37159-206">Login tooPantheon with admin credentials.</span></span>

2. <span data-ttu-id="37159-207">Keresse meg a túl**szervezet** irányítópult-oldalon.</span><span class="sxs-lookup"><span data-stu-id="37159-207">Navigate too**Organization** dashboard page.</span></span>
 
3. <span data-ttu-id="37159-208">Kattintson a **személyek**.</span><span class="sxs-lookup"><span data-stu-id="37159-208">Click **People**.</span></span>

4. <span data-ttu-id="37159-209">Kattintson a **felhasználó hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="37159-209">Click **Add user**.</span></span>

5. <span data-ttu-id="37159-210">Adja meg a hello a felhasználó e-mail címét.</span><span class="sxs-lookup"><span data-stu-id="37159-210">Enter hello user's email address.</span></span>

6. <span data-ttu-id="37159-211">Válassza ki a hello felhasználói szerepkört.</span><span class="sxs-lookup"><span data-stu-id="37159-211">Choose hello user's role.</span></span>

7. <span data-ttu-id="37159-212">Kattintson a **felhasználó hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="37159-212">Click **Add user**.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="37159-213">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="37159-213">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="37159-214">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooPantheon megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="37159-214">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooPantheon.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="37159-216">**tooassign Britta Simon tooPantheon, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="37159-216">**tooassign Britta Simon tooPantheon, perform hello following steps:**</span></span>

1. <span data-ttu-id="37159-217">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="37159-217">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="37159-219">Hello alkalmazások listában válassza ki a **Pantheon**.</span><span class="sxs-lookup"><span data-stu-id="37159-219">In hello applications list, select **Pantheon**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-pantheon-tutorial/tutorial_pantheon_app.png) 

3. <span data-ttu-id="37159-221">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="37159-221">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="37159-223">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="37159-223">Click **Add** button.</span></span> <span data-ttu-id="37159-224">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="37159-224">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="37159-226">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="37159-226">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="37159-227">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="37159-227">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="37159-228">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="37159-228">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="37159-229">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="37159-229">Testing single sign-on</span></span>

<span data-ttu-id="37159-230">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="37159-230">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="37159-231">Ha a hozzáférési Panel hello hello Pantheon csempe gombra kattint, automatikusan bejelentkezett tooyour Pantheon alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="37159-231">When you click hello Pantheon tile in hello Access Panel, you should get automatically signed-on tooyour Pantheon application.</span></span>
<span data-ttu-id="37159-232">További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="37159-232">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="37159-233">További források</span><span class="sxs-lookup"><span data-stu-id="37159-233">Additional resources</span></span>

* [<span data-ttu-id="37159-234">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="37159-234">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="37159-235">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="37159-235">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-pantheon-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-pantheon-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-pantheon-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-pantheon-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-pantheon-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-pantheon-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-pantheon-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-pantheon-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-pantheon-tutorial/tutorial_general_203.png

