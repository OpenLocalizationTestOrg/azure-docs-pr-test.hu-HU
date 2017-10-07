---
title: "Oktatóanyag: Azure Active Directoryval integrált EverBridge |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és EverBridge között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 58d7cd22-98c0-4606-9ce5-8bdb22ee8b3e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: a260298279407ed709bc2e685a104410f9836a74
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-everbridge"></a><span data-ttu-id="0e3ca-103">Oktatóanyag: Azure Active Directoryval integrált EverBridge</span><span class="sxs-lookup"><span data-stu-id="0e3ca-103">Tutorial: Azure Active Directory integration with EverBridge</span></span>

<span data-ttu-id="0e3ca-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate EverBridge az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="0e3ca-104">In this tutorial, you learn how toointegrate EverBridge with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="0e3ca-105">EverBridge integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="0e3ca-105">Integrating EverBridge with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="0e3ca-106">Megadhatja a hozzáférés tooEverBridge rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="0e3ca-106">You can control in Azure AD who has access tooEverBridge</span></span>
- <span data-ttu-id="0e3ca-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooEverBridge (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="0e3ca-107">You can enable your users tooautomatically get signed-on tooEverBridge (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="0e3ca-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="0e3ca-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="0e3ca-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="0e3ca-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0e3ca-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="0e3ca-110">Prerequisites</span></span>

<span data-ttu-id="0e3ca-111">az Azure AD integrálása EverBridge tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="0e3ca-111">tooconfigure Azure AD integration with EverBridge, you need hello following items:</span></span>

- <span data-ttu-id="0e3ca-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="0e3ca-112">An Azure AD subscription</span></span>
- <span data-ttu-id="0e3ca-113">Egy EverBridge egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="0e3ca-113">An EverBridge single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="0e3ca-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="0e3ca-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="0e3ca-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="0e3ca-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="0e3ca-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="0e3ca-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="0e3ca-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="0e3ca-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="0e3ca-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="0e3ca-118">Scenario description</span></span>
<span data-ttu-id="0e3ca-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="0e3ca-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="0e3ca-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="0e3ca-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="0e3ca-121">Hello gyűjteményből EverBridge hozzáadása</span><span class="sxs-lookup"><span data-stu-id="0e3ca-121">Adding EverBridge from hello gallery</span></span>
2. <span data-ttu-id="0e3ca-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="0e3ca-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-everbridge-from-hello-gallery"></a><span data-ttu-id="0e3ca-123">Hello gyűjteményből EverBridge hozzáadása</span><span class="sxs-lookup"><span data-stu-id="0e3ca-123">Adding EverBridge from hello gallery</span></span>
<span data-ttu-id="0e3ca-124">tooconfigure hello integrációja EverBridge az Azure AD-be, meg kell tooadd EverBridge hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="0e3ca-124">tooconfigure hello integration of EverBridge into Azure AD, you need tooadd EverBridge from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="0e3ca-125">**tooadd EverBridge hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="0e3ca-125">**tooadd EverBridge from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="0e3ca-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="0e3ca-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="0e3ca-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="0e3ca-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="0e3ca-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="0e3ca-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="0e3ca-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="0e3ca-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="0e3ca-133">Hello keresési mezőbe, írja be a **EverBridge**.</span><span class="sxs-lookup"><span data-stu-id="0e3ca-133">In hello search box, type **EverBridge**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-everbridge-tutorial/tutorial_everbridge_search.png)

5. <span data-ttu-id="0e3ca-135">A hello eredmények panelen válassza ki a **EverBridge**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="0e3ca-135">In hello results panel, select **EverBridge**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-everbridge-tutorial/tutorial_everbridge_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="0e3ca-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="0e3ca-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="0e3ca-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján EverBridge.</span><span class="sxs-lookup"><span data-stu-id="0e3ca-138">In this section, you configure and test Azure AD single sign-on with EverBridge based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="0e3ca-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó EverBridge tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="0e3ca-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in EverBridge is tooa user in Azure AD.</span></span> <span data-ttu-id="0e3ca-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello EverBridge közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="0e3ca-140">In other words, a link relationship between an Azure AD user and hello related user in EverBridge needs toobe established.</span></span>

<span data-ttu-id="0e3ca-141">EverBridge, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="0e3ca-141">In EverBridge, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="0e3ca-142">tooconfigure és az Azure AD az egyszeri bejelentkezés EverBridge-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="0e3ca-142">tooconfigure and test Azure AD single sign-on with EverBridge, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="0e3ca-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="0e3ca-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="0e3ca-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="0e3ca-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="0e3ca-145">**[Egy EverBridge tesztfelhasználó létrehozása](#creating-an-everbridge-test-user)**  -toohave egy megfelelője a Britta Simon a EverBridge, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="0e3ca-145">**[Creating an EverBridge test user](#creating-an-everbridge-test-user)** - toohave a counterpart of Britta Simon in EverBridge that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="0e3ca-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="0e3ca-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="0e3ca-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="0e3ca-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="0e3ca-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="0e3ca-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="0e3ca-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az EverBridge alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="0e3ca-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your EverBridge application.</span></span>

<span data-ttu-id="0e3ca-150">**az Azure AD tooconfigure egyszeri bejelentkezést a EverBridge, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="0e3ca-150">**tooconfigure Azure AD single sign-on with EverBridge, perform hello following steps:**</span></span>

1. <span data-ttu-id="0e3ca-151">Az Azure portál, a hello hello **EverBridge** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="0e3ca-151">In hello Azure portal, on hello **EverBridge** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="0e3ca-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="0e3ca-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-everbridge-tutorial/tutorial_everbridge_samlbase.png)

3. <span data-ttu-id="0e3ca-155">A hello **EverBridge tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="0e3ca-155">On hello **EverBridge Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-everbridge-tutorial/tutorial_everbridge_url.png)

    <span data-ttu-id="0e3ca-157">a.</span><span class="sxs-lookup"><span data-stu-id="0e3ca-157">a.</span></span> <span data-ttu-id="0e3ca-158">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://sso.everbridge.net/<companyname>`</span><span class="sxs-lookup"><span data-stu-id="0e3ca-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://sso.everbridge.net/<companyname>`</span></span>

    <span data-ttu-id="0e3ca-159">b.</span><span class="sxs-lookup"><span data-stu-id="0e3ca-159">b.</span></span> <span data-ttu-id="0e3ca-160">A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://manager.everbridge.net/saml/SSO/<companyname>/alias/defaultAlias`</span><span class="sxs-lookup"><span data-stu-id="0e3ca-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://manager.everbridge.net/saml/SSO/<companyname>/alias/defaultAlias`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="0e3ca-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="0e3ca-161">These values are not real.</span></span> <span data-ttu-id="0e3ca-162">Frissítheti ezeket az értékeket hello tényleges azonosítója és a válasz URL-címmel.</span><span class="sxs-lookup"><span data-stu-id="0e3ca-162">Update these values with hello actual Identifier and Reply URL.</span></span> <span data-ttu-id="0e3ca-163">Ügyfél [EverBridge támogatási csoport](mailto:support@everbridge.com) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="0e3ca-163">Contact [EverBridge support team](mailto:support@everbridge.com) tooget these values.</span></span>
 
4. <span data-ttu-id="0e3ca-164">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="0e3ca-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-everbridge-tutorial/tutorial_everbridge_certificate.png) 

5. <span data-ttu-id="0e3ca-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="0e3ca-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-everbridge-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="0e3ca-168">A hello **EverBridge konfigurációs** kattintson **konfigurálása EverBridge** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="0e3ca-168">On hello **EverBridge Configuration** section, click **Configure EverBridge** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="0e3ca-169">Másolás hello **SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="0e3ca-169">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-everbridge-tutorial/tutorial_everbridge_configure.png) 

6. <span data-ttu-id="0e3ca-171">tooget az alkalmazáshoz konfigurált SSO, toosign tooyour Everbridge bérlői rendszergazdaként kell.</span><span class="sxs-lookup"><span data-stu-id="0e3ca-171">tooget SSO configured for your application, you need toosign-on tooyour Everbridge tenant as an administrator.</span></span>

7. <span data-ttu-id="0e3ca-172">Hello hello felső menüben kattintson a hello **beállítások** lapot, és válasszon **egyszeri bejelentkezés** alatt **biztonsági**.</span><span class="sxs-lookup"><span data-stu-id="0e3ca-172">In hello menu on hello top, click hello **Settings** tab and select **Single Sign-On** under **Security**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-everbridge-tutorial/tutorial_everbridge_002.png)
   
    <span data-ttu-id="0e3ca-174">a.</span><span class="sxs-lookup"><span data-stu-id="0e3ca-174">a.</span></span> <span data-ttu-id="0e3ca-175">A hello **neve** szövegmezőjét azonosító szolgáltató hello nevét (például: a vállalat nevét).</span><span class="sxs-lookup"><span data-stu-id="0e3ca-175">In hello **Name** textbox, type hello name of Identifier Provider (for example: your company name).</span></span>
   
    <span data-ttu-id="0e3ca-176">b.</span><span class="sxs-lookup"><span data-stu-id="0e3ca-176">b.</span></span> <span data-ttu-id="0e3ca-177">A hello **API-név** szövegmezőhöz API hello nevét.</span><span class="sxs-lookup"><span data-stu-id="0e3ca-177">In hello **API Name** textbox, type hello name of API.</span></span>
   
    <span data-ttu-id="0e3ca-178">c.</span><span class="sxs-lookup"><span data-stu-id="0e3ca-178">c.</span></span> <span data-ttu-id="0e3ca-179">Kattintson a **Choose File** gomb tooupload hello metaadatait tartalmazó fájl, amely az Azure-portálról letöltött.</span><span class="sxs-lookup"><span data-stu-id="0e3ca-179">Click **Choose File** button tooupload hello metadata file which you downloaded from Azure portal.</span></span>
   
    <span data-ttu-id="0e3ca-180">d.</span><span class="sxs-lookup"><span data-stu-id="0e3ca-180">d.</span></span> <span data-ttu-id="0e3ca-181">Hello SAML-alapú identitás helyét, jelölje ki **identitás hello tulajdonos utasítás hello NameIdentifier elemében van**.</span><span class="sxs-lookup"><span data-stu-id="0e3ca-181">In hello SAML Identity Location, select **Identity is in hello NameIdentifier element of hello Subject statement**.</span></span>
   
    <span data-ttu-id="0e3ca-182">e.</span><span class="sxs-lookup"><span data-stu-id="0e3ca-182">e.</span></span> <span data-ttu-id="0e3ca-183">A hello **Identity Provider bejelentkezési URL-cím** szövegmezőhöz Beillesztés hello értékének SAML SSO URL-CÍMÉT az Azure AD.</span><span class="sxs-lookup"><span data-stu-id="0e3ca-183">In hello **Identity Provider Login URL** textbox, paste hello value of SAML SSO URL from Azure AD.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-everbridge-tutorial/tutorial_everbridge_003.png)
   
    <span data-ttu-id="0e3ca-185">f.</span><span class="sxs-lookup"><span data-stu-id="0e3ca-185">f.</span></span> <span data-ttu-id="0e3ca-186">A szolgáltató által kezdeményezett kérelem Szolgáltatáskötés hello, válassza ki **HTTP-átirányítás**.</span><span class="sxs-lookup"><span data-stu-id="0e3ca-186">In hello Service Provider Initiated Request Binding, select **HTTP Redirect**.</span></span>

    <span data-ttu-id="0e3ca-187">g.</span><span class="sxs-lookup"><span data-stu-id="0e3ca-187">g.</span></span> <span data-ttu-id="0e3ca-188">Kattintson a **mentése**</span><span class="sxs-lookup"><span data-stu-id="0e3ca-188">Click **Save**</span></span>

> [!TIP]
> <span data-ttu-id="0e3ca-189">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="0e3ca-189">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="0e3ca-190">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="0e3ca-190">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="0e3ca-191">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="0e3ca-191">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="0e3ca-192">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="0e3ca-192">Creating an Azure AD test user</span></span>
<span data-ttu-id="0e3ca-193">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="0e3ca-193">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="0e3ca-195">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="0e3ca-195">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="0e3ca-196">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="0e3ca-196">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-everbridge-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="0e3ca-198">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="0e3ca-198">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-everbridge-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="0e3ca-200">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="0e3ca-200">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-everbridge-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="0e3ca-202">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="0e3ca-202">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-everbridge-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="0e3ca-204">a.</span><span class="sxs-lookup"><span data-stu-id="0e3ca-204">a.</span></span> <span data-ttu-id="0e3ca-205">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="0e3ca-205">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="0e3ca-206">b.</span><span class="sxs-lookup"><span data-stu-id="0e3ca-206">b.</span></span> <span data-ttu-id="0e3ca-207">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="0e3ca-207">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="0e3ca-208">c.</span><span class="sxs-lookup"><span data-stu-id="0e3ca-208">c.</span></span> <span data-ttu-id="0e3ca-209">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="0e3ca-209">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="0e3ca-210">d.</span><span class="sxs-lookup"><span data-stu-id="0e3ca-210">d.</span></span> <span data-ttu-id="0e3ca-211">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="0e3ca-211">Click **Create**.</span></span>
 
### <a name="creating-an-everbridge-test-user"></a><span data-ttu-id="0e3ca-212">Egy EverBridge tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="0e3ca-212">Creating an EverBridge test user</span></span>

<span data-ttu-id="0e3ca-213">Ebben a szakaszban egy Everbridge Britta Simon nevű felhasználót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="0e3ca-213">In this section, you create a user called Britta Simon in Everbridge.</span></span> <span data-ttu-id="0e3ca-214">Együttműködve [EverBridge támogatási csoport](mailto:support@everbridge.com) tooadd hello felhasználók hello Everbridge platform.</span><span class="sxs-lookup"><span data-stu-id="0e3ca-214">Work with [EverBridge support team](mailto:support@everbridge.com) tooadd hello users in hello Everbridge platform.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="0e3ca-215">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="0e3ca-215">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="0e3ca-216">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooEverBridge megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="0e3ca-216">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooEverBridge.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="0e3ca-218">**tooassign Britta Simon tooEverBridge, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="0e3ca-218">**tooassign Britta Simon tooEverBridge, perform hello following steps:**</span></span>

1. <span data-ttu-id="0e3ca-219">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="0e3ca-219">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="0e3ca-221">Hello alkalmazások listában válassza ki a **EverBridge**.</span><span class="sxs-lookup"><span data-stu-id="0e3ca-221">In hello applications list, select **EverBridge**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-everbridge-tutorial/tutorial_everbridge_app.png) 

3. <span data-ttu-id="0e3ca-223">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="0e3ca-223">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="0e3ca-225">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="0e3ca-225">Click **Add** button.</span></span> <span data-ttu-id="0e3ca-226">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="0e3ca-226">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="0e3ca-228">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="0e3ca-228">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="0e3ca-229">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="0e3ca-229">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="0e3ca-230">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="0e3ca-230">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="0e3ca-231">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="0e3ca-231">Testing single sign-on</span></span>

<span data-ttu-id="0e3ca-232">hello ebben a szakaszban célja tootest az egyszeri bejelentkezés konfigurációs használatával hello a hozzáférési Panel.</span><span class="sxs-lookup"><span data-stu-id="0e3ca-232">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="0e3ca-233">Ha a hozzáférési Panel hello hello Everbridge csempe gombra kattint, automatikusan bejelentkezett tooyour Everbridge alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="0e3ca-233">When you click hello Everbridge tile in hello Access Panel, you should get automatically signed-on tooyour Everbridge application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0e3ca-234">További források</span><span class="sxs-lookup"><span data-stu-id="0e3ca-234">Additional resources</span></span>

* [<span data-ttu-id="0e3ca-235">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="0e3ca-235">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="0e3ca-236">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="0e3ca-236">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_203.png

