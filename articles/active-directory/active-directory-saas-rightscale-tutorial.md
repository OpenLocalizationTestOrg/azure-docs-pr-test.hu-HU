---
title: "Oktatóanyag: Azure Active Directoryval integrált Rightscale |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Rightscale között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 3a8d376d-95fb-4dd7-832a-4fdd4dd7c87c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2017
ms.author: jeedes
ms.openlocfilehash: 53b927804a1e0f895778a164386459a4ea816f98
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-rightscale"></a><span data-ttu-id="11b44-103">Oktatóanyag: Azure Active Directoryval integrált Rightscale</span><span class="sxs-lookup"><span data-stu-id="11b44-103">Tutorial: Azure Active Directory integration with Rightscale</span></span>

<span data-ttu-id="11b44-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Rightscale az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="11b44-104">In this tutorial, you learn how toointegrate Rightscale with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="11b44-105">Rightscale integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="11b44-105">Integrating Rightscale with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="11b44-106">Megadhatja a hozzáférés tooRightscale rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="11b44-106">You can control in Azure AD who has access tooRightscale</span></span>
- <span data-ttu-id="11b44-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooRightscale (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="11b44-107">You can enable your users tooautomatically get signed-on tooRightscale (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="11b44-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="11b44-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="11b44-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="11b44-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="11b44-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="11b44-110">Prerequisites</span></span>

<span data-ttu-id="11b44-111">az Azure AD integrálása Rightscale tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="11b44-111">tooconfigure Azure AD integration with Rightscale, you need hello following items:</span></span>

- <span data-ttu-id="11b44-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="11b44-112">An Azure AD subscription</span></span>
- <span data-ttu-id="11b44-113">Egy Rightscale egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="11b44-113">A Rightscale single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="11b44-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="11b44-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="11b44-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="11b44-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="11b44-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="11b44-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="11b44-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="11b44-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="11b44-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="11b44-118">Scenario description</span></span>
<span data-ttu-id="11b44-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="11b44-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="11b44-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="11b44-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="11b44-121">Hello gyűjteményből Rightscale hozzáadása</span><span class="sxs-lookup"><span data-stu-id="11b44-121">Adding Rightscale from hello gallery</span></span>
2. <span data-ttu-id="11b44-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="11b44-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-rightscale-from-hello-gallery"></a><span data-ttu-id="11b44-123">Hello gyűjteményből Rightscale hozzáadása</span><span class="sxs-lookup"><span data-stu-id="11b44-123">Adding Rightscale from hello gallery</span></span>
<span data-ttu-id="11b44-124">tooconfigure hello integrációja Rightscale az Azure AD-be, meg kell tooadd Rightscale hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="11b44-124">tooconfigure hello integration of Rightscale into Azure AD, you need tooadd Rightscale from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="11b44-125">**tooadd Rightscale hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="11b44-125">**tooadd Rightscale from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="11b44-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="11b44-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="11b44-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="11b44-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="11b44-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="11b44-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="11b44-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="11b44-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="11b44-133">Hello keresési mezőbe, írja be a **Rightscale**.</span><span class="sxs-lookup"><span data-stu-id="11b44-133">In hello search box, type **Rightscale**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_search.png)

5. <span data-ttu-id="11b44-135">A hello eredmények panelen válassza ki a **Rightscale**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="11b44-135">In hello results panel, select **Rightscale**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="11b44-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="11b44-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="11b44-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Rightscale.</span><span class="sxs-lookup"><span data-stu-id="11b44-138">In this section, you configure and test Azure AD single sign-on with Rightscale based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="11b44-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Rightscale tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="11b44-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Rightscale is tooa user in Azure AD.</span></span> <span data-ttu-id="11b44-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Rightscale közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="11b44-140">In other words, a link relationship between an Azure AD user and hello related user in Rightscale needs toobe established.</span></span>

<span data-ttu-id="11b44-141">Rightscale, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="11b44-141">In Rightscale, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="11b44-142">tooconfigure és az Azure AD az egyszeri bejelentkezés Rightscale-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="11b44-142">tooconfigure and test Azure AD single sign-on with Rightscale, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="11b44-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="11b44-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="11b44-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="11b44-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="11b44-145">**[Rightscale tesztfelhasználó létrehozása](#creating-a-rightscale-test-user)**  -toohave egy megfelelője a Britta Simon a Rightscale, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="11b44-145">**[Creating a Rightscale test user](#creating-a-rightscale-test-user)** - toohave a counterpart of Britta Simon in Rightscale that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="11b44-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="11b44-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="11b44-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="11b44-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="11b44-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="11b44-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="11b44-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Rightscale alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="11b44-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Rightscale application.</span></span>

<span data-ttu-id="11b44-150">**az Azure AD tooconfigure egyszeri bejelentkezést a Rightscale, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="11b44-150">**tooconfigure Azure AD single sign-on with Rightscale, perform hello following steps:**</span></span>

1. <span data-ttu-id="11b44-151">Az Azure portál, a hello hello **Rightscale** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="11b44-151">In hello Azure portal, on hello **Rightscale** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="11b44-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="11b44-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_samlbase.png)

3. <span data-ttu-id="11b44-155">A hello **Rightscale tartomány és az URL-címek** szakaszban, ha tooconfigure hello alkalmazás **IDP kezdeményezett mód** nincs tooperform olyan lépéseket, hello alkalmazás már előre integrálva van az Azure-ral.</span><span class="sxs-lookup"><span data-stu-id="11b44-155">On hello **Rightscale Domain and URLs** section, if you wish tooconfigure hello application in **IDP initiated mode** you do not have tooperform any steps as hello app is already pre-integrated with Azure.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_url.png)

4. <span data-ttu-id="11b44-157">A hello **Rightscale tartomány és az URL-címek** szakaszban, ha tooconfigure hello alkalmazás **Szolgáltató kezdeményezett mód**, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="11b44-157">On hello **Rightscale Domain and URLs** section, if you wish tooconfigure hello application in **SP initiated mode**, perform hello following steps:</span></span>
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_url1.png)

    <span data-ttu-id="11b44-159">a.</span><span class="sxs-lookup"><span data-stu-id="11b44-159">a.</span></span> <span data-ttu-id="11b44-160">Kattintson a hello **megjelenítése speciális URL-beállításainak**.</span><span class="sxs-lookup"><span data-stu-id="11b44-160">Click on hello **Show advanced URL settings**.</span></span>

    <span data-ttu-id="11b44-161">b.</span><span class="sxs-lookup"><span data-stu-id="11b44-161">b.</span></span> <span data-ttu-id="11b44-162">A hello **URL-cím bejelentkezési** szövegmezőhöz típus hello URL-címe:`https://login.rightscale.com/`</span><span class="sxs-lookup"><span data-stu-id="11b44-162">In hello **Sign On URL** textbox, type hello URL: `https://login.rightscale.com/`</span></span>

5. <span data-ttu-id="11b44-163">A hello **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="11b44-163">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_certificate.png) 

6. <span data-ttu-id="11b44-165">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="11b44-165">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-rightscale-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="11b44-167">A hello **Rightscale konfigurációs** kattintson **konfigurálása Rightscale** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="11b44-167">On hello **Rightscale Configuration** section, click **Configure Rightscale** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="11b44-168">Másolás hello **SAML Entitásazonosító és SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="11b44-168">Copy hello **SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    <span data-ttu-id="11b44-169">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_configure.png) 
<CS></span><span class="sxs-lookup"><span data-stu-id="11b44-169">![Configure Single Sign-On](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_configure.png) 
<CS></span></span>
8. <span data-ttu-id="11b44-170">tooget az alkalmazáshoz konfigurált SSO, toosign tooyour RightScale bérlői rendszergazdaként kell.</span><span class="sxs-lookup"><span data-stu-id="11b44-170">tooget SSO configured for your application, you need toosign-on tooyour RightScale tenant as an administrator.</span></span>

    <span data-ttu-id="11b44-171">a.</span><span class="sxs-lookup"><span data-stu-id="11b44-171">a.</span></span> <span data-ttu-id="11b44-172">Hello hello felső menüben kattintson a hello **beállítások** lapot, és válasszon **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="11b44-172">In hello menu on hello top, click hello **Settings** tab and select **Single Sign-On**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_001.png) 

    <span data-ttu-id="11b44-174">b.</span><span class="sxs-lookup"><span data-stu-id="11b44-174">b.</span></span> <span data-ttu-id="11b44-175">Kattintson a hello "**új**" gomb tooadd **a SAML-alapú identitás-szolgáltatóktól**.</span><span class="sxs-lookup"><span data-stu-id="11b44-175">Click hello "**new**" button tooadd **Your SAML Identity Providers**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_002.png) 
 
    <span data-ttu-id="11b44-177">c.</span><span class="sxs-lookup"><span data-stu-id="11b44-177">c.</span></span> <span data-ttu-id="11b44-178">A hello szövegmezőjének **megjelenített név**, adjon meg a cég nevét.</span><span class="sxs-lookup"><span data-stu-id="11b44-178">In hello textbox of **Display Name**, input your company name.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_003.png)
 
    <span data-ttu-id="11b44-180">d.</span><span class="sxs-lookup"><span data-stu-id="11b44-180">d.</span></span> <span data-ttu-id="11b44-181">Válassza ki **engedélyezése RightScale által kezdeményezett egyszeri Bejelentkezést használó felderítés mutató** bemeneti és a **tartománynév** hello szövegmező alatt található.</span><span class="sxs-lookup"><span data-stu-id="11b44-181">Select **Allow RightScale-initiated SSO using a discovery hint** and input your **domain name** in hello below textbox.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_004.png)

    <span data-ttu-id="11b44-183">e.</span><span class="sxs-lookup"><span data-stu-id="11b44-183">e.</span></span> <span data-ttu-id="11b44-184">Illessze be a hello értékének **SAML-alapú egyszeri bejelentkezési URL-címe** , amely az Azure portálról másolta **SAML SSO végpont** a RightScale.</span><span class="sxs-lookup"><span data-stu-id="11b44-184">Paste hello value of **SAML Single Sign-On Service URL** which you have copied from Azure portal into **SAML SSO Endpoint** in RightScale.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_006.png)

    <span data-ttu-id="11b44-186">f.</span><span class="sxs-lookup"><span data-stu-id="11b44-186">f.</span></span> <span data-ttu-id="11b44-187">Illessze be a hello értékének **SAML Entitásazonosító** , amely az Azure portálról másolta **SAML entityid beállítást** a RightScale.</span><span class="sxs-lookup"><span data-stu-id="11b44-187">Paste hello value of **SAML Entity ID** which you have copied from Azure portal into **SAML EntityID** in RightScale.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_008.png)

    <span data-ttu-id="11b44-189">g.</span><span class="sxs-lookup"><span data-stu-id="11b44-189">g.</span></span> <span data-ttu-id="11b44-190">Kattintson a **böngésző** gomb tooupload hello tanúsítvány, amely az Azure-portálról letöltött.</span><span class="sxs-lookup"><span data-stu-id="11b44-190">Click **Browser** button tooupload hello certificate which you downloaded from Azure portal.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_009.png)

    <span data-ttu-id="11b44-192">h.</span><span class="sxs-lookup"><span data-stu-id="11b44-192">h.</span></span> <span data-ttu-id="11b44-193">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="11b44-193">Click **Save**.</span></span>
<CE>
> [!TIP]
> <span data-ttu-id="11b44-194">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="11b44-194">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="11b44-195">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="11b44-195">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="11b44-196">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="11b44-196">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="11b44-197">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="11b44-197">Creating an Azure AD test user</span></span>
<span data-ttu-id="11b44-198">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="11b44-198">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="11b44-200">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="11b44-200">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="11b44-201">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="11b44-201">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-rightscale-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="11b44-203">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="11b44-203">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-rightscale-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="11b44-205">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="11b44-205">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-rightscale-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="11b44-207">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="11b44-207">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-rightscale-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="11b44-209">a.</span><span class="sxs-lookup"><span data-stu-id="11b44-209">a.</span></span> <span data-ttu-id="11b44-210">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="11b44-210">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="11b44-211">b.</span><span class="sxs-lookup"><span data-stu-id="11b44-211">b.</span></span> <span data-ttu-id="11b44-212">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="11b44-212">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="11b44-213">c.</span><span class="sxs-lookup"><span data-stu-id="11b44-213">c.</span></span> <span data-ttu-id="11b44-214">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="11b44-214">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="11b44-215">d.</span><span class="sxs-lookup"><span data-stu-id="11b44-215">d.</span></span> <span data-ttu-id="11b44-216">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="11b44-216">Click **Create**.</span></span>
 
### <a name="creating-a-rightscale-test-user"></a><span data-ttu-id="11b44-217">Rightscale tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="11b44-217">Creating a Rightscale test user</span></span>

<span data-ttu-id="11b44-218">Ebben a szakaszban egy RightScale Britta Simon nevű felhasználót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="11b44-218">In this section, you create a user called Britta Simon in RightScale.</span></span> <span data-ttu-id="11b44-219">Együttműködve [Rightscale ügyfél-támogatási csoport](mailto:support@rightscale.com) tooadd hello felhasználók hello RightScale platform.</span><span class="sxs-lookup"><span data-stu-id="11b44-219">Work with [Rightscale Client support team](mailto:support@rightscale.com) tooadd hello users in hello RightScale platform.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="11b44-220">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="11b44-220">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="11b44-221">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooRightscale megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="11b44-221">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooRightscale.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="11b44-223">**tooassign Britta Simon tooRightscale, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="11b44-223">**tooassign Britta Simon tooRightscale, perform hello following steps:**</span></span>

1. <span data-ttu-id="11b44-224">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="11b44-224">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="11b44-226">Hello alkalmazások listában válassza ki a **Rightscale**.</span><span class="sxs-lookup"><span data-stu-id="11b44-226">In hello applications list, select **Rightscale**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_app.png) 

3. <span data-ttu-id="11b44-228">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="11b44-228">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="11b44-230">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="11b44-230">Click **Add** button.</span></span> <span data-ttu-id="11b44-231">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="11b44-231">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="11b44-233">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="11b44-233">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="11b44-234">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="11b44-234">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="11b44-235">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="11b44-235">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="11b44-236">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="11b44-236">Testing single sign-on</span></span>

<span data-ttu-id="11b44-237">hello ebben a szakaszban célja tootest hozzáférési Panel az Azure AD SSO konfigurációs használatával hello.</span><span class="sxs-lookup"><span data-stu-id="11b44-237">hello objective of this section is tootest your Azure AD SSO configuration using hello Access Panel.</span></span>  

<span data-ttu-id="11b44-238">Ha a hozzáférési Panel hello hello RightScale csempe gombra kattint, automatikusan bejelentkezett tooyour RightScale alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="11b44-238">When you click hello RightScale tile in hello Access Panel, you should get automatically signed-on tooyour RightScale application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="11b44-239">További források</span><span class="sxs-lookup"><span data-stu-id="11b44-239">Additional resources</span></span>

* [<span data-ttu-id="11b44-240">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="11b44-240">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="11b44-241">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="11b44-241">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_203.png

