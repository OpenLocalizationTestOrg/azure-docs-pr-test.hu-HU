---
title: "Oktatóanyag: Azure Active Directoryval integrált Hightail |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Hightail között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: e15206ac-74b0-46e4-9329-892c7d242ec0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/21/2017
ms.author: jeedes
ms.openlocfilehash: 2b36fcf8d5773255fdf89de2dccdceb95c032bd8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-hightail"></a><span data-ttu-id="d0b50-103">Oktatóanyag: Azure Active Directoryval integrált Hightail</span><span class="sxs-lookup"><span data-stu-id="d0b50-103">Tutorial: Azure Active Directory integration with Hightail</span></span>

<span data-ttu-id="d0b50-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Hightail az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="d0b50-104">In this tutorial, you learn how toointegrate Hightail with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="d0b50-105">Hightail integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="d0b50-105">Integrating Hightail with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="d0b50-106">Megadhatja a hozzáférés tooHightail rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="d0b50-106">You can control in Azure AD who has access tooHightail</span></span>
- <span data-ttu-id="d0b50-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooHightail (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="d0b50-107">You can enable your users tooautomatically get signed-on tooHightail (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="d0b50-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="d0b50-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="d0b50-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="d0b50-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d0b50-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="d0b50-110">Prerequisites</span></span>

<span data-ttu-id="d0b50-111">az Azure AD integrálása Hightail tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="d0b50-111">tooconfigure Azure AD integration with Hightail, you need hello following items:</span></span>

- <span data-ttu-id="d0b50-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="d0b50-112">An Azure AD subscription</span></span>
- <span data-ttu-id="d0b50-113">Egy Hightail egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="d0b50-113">A Hightail single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="d0b50-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="d0b50-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="d0b50-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="d0b50-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="d0b50-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="d0b50-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="d0b50-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="d0b50-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="d0b50-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="d0b50-118">Scenario description</span></span>
<span data-ttu-id="d0b50-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="d0b50-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="d0b50-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="d0b50-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="d0b50-121">Hello gyűjteményből Hightail hozzáadása</span><span class="sxs-lookup"><span data-stu-id="d0b50-121">Adding Hightail from hello gallery</span></span>
2. <span data-ttu-id="d0b50-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="d0b50-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-hightail-from-hello-gallery"></a><span data-ttu-id="d0b50-123">Hello gyűjteményből Hightail hozzáadása</span><span class="sxs-lookup"><span data-stu-id="d0b50-123">Adding Hightail from hello gallery</span></span>
<span data-ttu-id="d0b50-124">tooconfigure hello integrációja Hightail az Azure AD-be, meg kell tooadd Hightail hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="d0b50-124">tooconfigure hello integration of Hightail into Azure AD, you need tooadd Hightail from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="d0b50-125">**tooadd Hightail hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="d0b50-125">**tooadd Hightail from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="d0b50-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="d0b50-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="d0b50-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="d0b50-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="d0b50-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="d0b50-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="d0b50-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="d0b50-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="d0b50-133">Hello keresési mezőbe, írja be a **Hightail**.</span><span class="sxs-lookup"><span data-stu-id="d0b50-133">In hello search box, type **Hightail**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_search.png)

5. <span data-ttu-id="d0b50-135">A hello eredmények panelen válassza ki a **Hightail**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="d0b50-135">In hello results panel, select **Hightail**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="d0b50-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="d0b50-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="d0b50-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Hightail.</span><span class="sxs-lookup"><span data-stu-id="d0b50-138">In this section, you configure and test Azure AD single sign-on with Hightail based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="d0b50-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Hightail tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="d0b50-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Hightail is tooa user in Azure AD.</span></span> <span data-ttu-id="d0b50-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Hightail közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="d0b50-140">In other words, a link relationship between an Azure AD user and hello related user in Hightail needs toobe established.</span></span>

<span data-ttu-id="d0b50-141">Hightail, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="d0b50-141">In Hightail, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="d0b50-142">tooconfigure és az Azure AD az egyszeri bejelentkezés Hightail-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="d0b50-142">tooconfigure and test Azure AD single sign-on with Hightail, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="d0b50-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="d0b50-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="d0b50-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d0b50-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="d0b50-145">**[Hightail tesztfelhasználó létrehozása](#creating-a-hightail-test-user)**  -toohave egy megfelelője a Britta Simon a Hightail, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="d0b50-145">**[Creating a Hightail test user](#creating-a-hightail-test-user)** - toohave a counterpart of Britta Simon in Hightail that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="d0b50-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="d0b50-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="d0b50-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="d0b50-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="d0b50-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="d0b50-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="d0b50-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Hightail alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="d0b50-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Hightail application.</span></span>

<span data-ttu-id="d0b50-150">**az Azure AD tooconfigure egyszeri bejelentkezést a Hightail, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="d0b50-150">**tooconfigure Azure AD single sign-on with Hightail, perform hello following steps:**</span></span>

1. <span data-ttu-id="d0b50-151">Az Azure portál, a hello hello **Hightail** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="d0b50-151">In hello Azure portal, on hello **Hightail** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="d0b50-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="d0b50-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_samlbase.png)

3. <span data-ttu-id="d0b50-155">A hello **Hightail tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="d0b50-155">On hello **Hightail Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_url.png)

     <span data-ttu-id="d0b50-157">A hello **válasz URL-CÍMEN** szövegmezőhöz típus hello az URL-címet:`https://www.hightail.com/samlLogin?phi_action=app/samlLogin&subAction=handleSamlResponse`</span><span class="sxs-lookup"><span data-stu-id="d0b50-157">In hello **Reply URL** textbox, type hello URL as: `https://www.hightail.com/samlLogin?phi_action=app/samlLogin&subAction=handleSamlResponse`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="d0b50-158">hello előző érték nincs valós értékének.</span><span class="sxs-lookup"><span data-stu-id="d0b50-158">hello preceding value is not real value.</span></span> <span data-ttu-id="d0b50-159">Hello érték hello tényleges válasz URL-CÍMEN, hello oktatóanyag későbbi részében ismertetett frissíti.</span><span class="sxs-lookup"><span data-stu-id="d0b50-159">You will update hello value with hello actual Reply URL, which is explained later in hello tutorial.</span></span>
 
4. <span data-ttu-id="d0b50-160">A hello **Hightail tartomány és az URL-címek** szakaszban, ha tooconfigure hello alkalmazás **Szolgáltató kezdeményezett mód**, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="d0b50-160">On hello **Hightail Domain and URLs** section, If you wish tooconfigure hello application in **SP initiated mode**, perform hello following steps:</span></span>
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_url1.png)

    <span data-ttu-id="d0b50-162">a.</span><span class="sxs-lookup"><span data-stu-id="d0b50-162">a.</span></span> <span data-ttu-id="d0b50-163">Kattintson a hello **megjelenítése speciális URL-beállításainak**.</span><span class="sxs-lookup"><span data-stu-id="d0b50-163">Click hello **Show advanced URL settings**.</span></span>

    <span data-ttu-id="d0b50-164">b.</span><span class="sxs-lookup"><span data-stu-id="d0b50-164">b.</span></span> <span data-ttu-id="d0b50-165">A hello **URL-cím bejelentkezési** szövegmezőhöz típus hello az URL-címet:`https://www.hightail.com/loginSSO`</span><span class="sxs-lookup"><span data-stu-id="d0b50-165">In hello **Sign On URL** textbox, type hello URL as: `https://www.hightail.com/loginSSO`</span></span>

4. <span data-ttu-id="d0b50-166">A hello **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="d0b50-166">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_certificate.png) 

5. <span data-ttu-id="d0b50-168">Hightail alkalmazás hello SAML helyességi feltételek vár egy meghatározott formátumban.</span><span class="sxs-lookup"><span data-stu-id="d0b50-168">Hightail application expects hello SAML assertions in a specific format.</span></span> <span data-ttu-id="d0b50-169">Állítsa be az alkalmazás jogcímek a következő hello.</span><span class="sxs-lookup"><span data-stu-id="d0b50-169">Please configure hello following claims for this application.</span></span> <span data-ttu-id="d0b50-170">Ezek az attribútumok értékének hello kezelheti hello **"Atrribute"** hello alkalmazás lapján.</span><span class="sxs-lookup"><span data-stu-id="d0b50-170">You can manage hello values of these attributes from hello **"Atrribute"** tab of hello application.</span></span> <span data-ttu-id="d0b50-171">a következő képernyőkép hello ezen mutat egy példát.</span><span class="sxs-lookup"><span data-stu-id="d0b50-171">hello following screenshot shows an example for this.</span></span> 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_attribute.png) 

6. <span data-ttu-id="d0b50-173">A hello **felhasználói attribútumok** szakaszt, hello **egyszeri bejelentkezés** párbeszédpanelen konfigurálja a SAML-jogkivonat attribútum hello ábrának megfelelően, és hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="d0b50-173">In hello **User Attributes** section on hello **Single sign-on** dialog, configure SAML token attribute as shown in hello image and perform hello following steps:</span></span>
    
    | <span data-ttu-id="d0b50-174">Attribútum neve</span><span class="sxs-lookup"><span data-stu-id="d0b50-174">Attribute Name</span></span> | <span data-ttu-id="d0b50-175">Attribútum-érték</span><span class="sxs-lookup"><span data-stu-id="d0b50-175">Attribute Value</span></span> |
    | ------------------- | -------------------- |
    | <span data-ttu-id="d0b50-176">Utónév</span><span class="sxs-lookup"><span data-stu-id="d0b50-176">FirstName</span></span> | <span data-ttu-id="d0b50-177">User.givenName</span><span class="sxs-lookup"><span data-stu-id="d0b50-177">user.givenname</span></span> |
    | <span data-ttu-id="d0b50-178">Vezetéknév</span><span class="sxs-lookup"><span data-stu-id="d0b50-178">LastName</span></span> | <span data-ttu-id="d0b50-179">User.surname</span><span class="sxs-lookup"><span data-stu-id="d0b50-179">user.surname</span></span> |
    | <span data-ttu-id="d0b50-180">E-mail cím</span><span class="sxs-lookup"><span data-stu-id="d0b50-180">Email</span></span> | <span data-ttu-id="d0b50-181">User.mail</span><span class="sxs-lookup"><span data-stu-id="d0b50-181">user.mail</span></span> |    
    | <span data-ttu-id="d0b50-182">UserIdentity</span><span class="sxs-lookup"><span data-stu-id="d0b50-182">UserIdentity</span></span> | <span data-ttu-id="d0b50-183">User.mail</span><span class="sxs-lookup"><span data-stu-id="d0b50-183">user.mail</span></span> |
    
    <span data-ttu-id="d0b50-184">a.</span><span class="sxs-lookup"><span data-stu-id="d0b50-184">a.</span></span> <span data-ttu-id="d0b50-185">Kattintson a **Hozzáadás attribútum** tooopen hello **attribútum hozzáadása** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d0b50-185">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hightail-tutorial/tutorial_officespace_04.png)

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hightail-tutorial/tutorial_officespace_05.png)

    <span data-ttu-id="d0b50-188">b.</span><span class="sxs-lookup"><span data-stu-id="d0b50-188">b.</span></span> <span data-ttu-id="d0b50-189">A hello **neve** szövegmezőben, az adott sorhoz feltüntetett hello attribútum neve.</span><span class="sxs-lookup"><span data-stu-id="d0b50-189">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>

    <span data-ttu-id="d0b50-190">c.</span><span class="sxs-lookup"><span data-stu-id="d0b50-190">c.</span></span> <span data-ttu-id="d0b50-191">A hello **érték** listájában, hello attribútuma Típusérték az adott sorhoz feltüntetett.</span><span class="sxs-lookup"><span data-stu-id="d0b50-191">From hello **Value** list, type hello attribute value shown for that row.</span></span>

    <span data-ttu-id="d0b50-192">d.</span><span class="sxs-lookup"><span data-stu-id="d0b50-192">d.</span></span> <span data-ttu-id="d0b50-193">Hagyja hello **Namespace** üres.</span><span class="sxs-lookup"><span data-stu-id="d0b50-193">Leave hello **Namespace** blank.</span></span>
    
    <span data-ttu-id="d0b50-194">e.</span><span class="sxs-lookup"><span data-stu-id="d0b50-194">e.</span></span> <span data-ttu-id="d0b50-195">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="d0b50-195">Click **Ok**.</span></span>

7. <span data-ttu-id="d0b50-196">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="d0b50-196">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hightail-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="d0b50-198">A hello **Hightail konfigurációs** kattintson **konfigurálása Hightail** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="d0b50-198">On hello **Hightail Configuration** section, click **Configure Hightail** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="d0b50-199">Másolás hello **SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="d0b50-199">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_configure.png) 

    >[!NOTE] 
    ><span data-ttu-id="d0b50-201">Egyszeri bejelentkezés hello konfigurálása: Hightail app, előtt adja a Hightail e-mail tartomány vonja össze az, hogy a felhasználóknak, akik a tartományi hello összes fehér lista szolgáltatásával egyszeri bejelentkezés.</span><span class="sxs-lookup"><span data-stu-id="d0b50-201">Before configuring hello Single Sign On at Hightail app, please white list your email domain with Hightail team so that all hello users who are using this domain can use Single Sign On functionality.</span></span>


9. <span data-ttu-id="d0b50-202">tooget az alkalmazáshoz konfigurált SSO, toosign tooyour Hightail bérlői rendszergazdaként kell.</span><span class="sxs-lookup"><span data-stu-id="d0b50-202">tooget SSO configured for your application, you need toosign-on tooyour Hightail tenant as an administrator.</span></span>
   
    <span data-ttu-id="d0b50-203">a.</span><span class="sxs-lookup"><span data-stu-id="d0b50-203">a.</span></span> <span data-ttu-id="d0b50-204">Hello hello felső menüben kattintson a hello **fiók** lapot, és válasszon **SAML konfigurálása**.</span><span class="sxs-lookup"><span data-stu-id="d0b50-204">In hello menu on hello top, click hello **Account** tab and select **Configure SAML**.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_001.png) 

    <span data-ttu-id="d0b50-206">b.</span><span class="sxs-lookup"><span data-stu-id="d0b50-206">b.</span></span> <span data-ttu-id="d0b50-207">Válassza ki a hello jelölőnégyzetet az **SAML-alapú hitelesítés engedélyezéséhez**.</span><span class="sxs-lookup"><span data-stu-id="d0b50-207">Select hello checkbox of **Enable SAML Authentication**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_002.png) 

    <span data-ttu-id="d0b50-209">c.</span><span class="sxs-lookup"><span data-stu-id="d0b50-209">c.</span></span> <span data-ttu-id="d0b50-210">A base-64 kódolású tanúsítvány megnyitása a Jegyzettömbben az Azure portálról, a vágólapra tartalmának másolása hello letöltött és toohello Beillesztés **SAML jogkivonat-aláíró tanúsítványa** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="d0b50-210">Open your base-64 encoded certificate in notepad downloaded from Azure portal, copy hello content of it into your clipboard, and then paste it toohello **SAML Token Signing Certificate** textbox.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_003.png) 

    <span data-ttu-id="d0b50-212">d.</span><span class="sxs-lookup"><span data-stu-id="d0b50-212">d.</span></span> <span data-ttu-id="d0b50-213">A hello **SAML hatóság (identitásszolgáltató)** szövegmezőhöz Beillesztés hello értékének **SAML-alapú egyszeri bejelentkezési URL-címe** átmásolja az Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="d0b50-213">In hello **SAML Authority (Identity Provider)** textbox, paste hello value of **SAML Single Sign-On Service URL** copied from Azure portal.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_004.png)

    <span data-ttu-id="d0b50-215">e.</span><span class="sxs-lookup"><span data-stu-id="d0b50-215">e.</span></span> <span data-ttu-id="d0b50-216">Ha tooconfigure hello alkalmazás **IDP kezdeményezett mód** válasszon **"Identitásszolgáltató (IdP) kezdeményezett bejelentkezés"**.</span><span class="sxs-lookup"><span data-stu-id="d0b50-216">If you wish tooconfigure hello application in **IDP initiated mode** select **"Identity Provider (IdP) initiated log in"**.</span></span> <span data-ttu-id="d0b50-217">Ha **Szolgáltató kezdeményezett mód** válasszon **"Szolgáltatókkal (SP) kezdeményezett bejelentkezés"**.</span><span class="sxs-lookup"><span data-stu-id="d0b50-217">If **SP initiated mode** select **"Service Provider (SP) initiated log in"**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_006.png)

    <span data-ttu-id="d0b50-219">f.</span><span class="sxs-lookup"><span data-stu-id="d0b50-219">f.</span></span> <span data-ttu-id="d0b50-220">Hello SAML-alapú ügyfél URL-címe a példány másolja és illessze be azt a **válasz URL-CÍMEN** textbox **Hightail tartomány és az URL-címek** Azure-portál szakaszban.</span><span class="sxs-lookup"><span data-stu-id="d0b50-220">Copy hello SAML consumer URL for your instance and paste it in **Reply URL** textbox in **Hightail Domain and URLs** section on Azure portal.</span></span>
    
    <span data-ttu-id="d0b50-221">g.</span><span class="sxs-lookup"><span data-stu-id="d0b50-221">g.</span></span> <span data-ttu-id="d0b50-222">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="d0b50-222">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="d0b50-223">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="d0b50-223">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="d0b50-224">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="d0b50-224">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="d0b50-225">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="d0b50-225">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="d0b50-226">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="d0b50-226">Creating an Azure AD test user</span></span>
<span data-ttu-id="d0b50-227">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="d0b50-227">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="d0b50-229">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="d0b50-229">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="d0b50-230">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="d0b50-230">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-hightail-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="d0b50-232">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="d0b50-232">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-hightail-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="d0b50-234">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d0b50-234">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-hightail-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="d0b50-236">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="d0b50-236">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-hightail-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="d0b50-238">a.</span><span class="sxs-lookup"><span data-stu-id="d0b50-238">a.</span></span> <span data-ttu-id="d0b50-239">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="d0b50-239">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="d0b50-240">b.</span><span class="sxs-lookup"><span data-stu-id="d0b50-240">b.</span></span> <span data-ttu-id="d0b50-241">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="d0b50-241">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="d0b50-242">c.</span><span class="sxs-lookup"><span data-stu-id="d0b50-242">c.</span></span> <span data-ttu-id="d0b50-243">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="d0b50-243">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="d0b50-244">d.</span><span class="sxs-lookup"><span data-stu-id="d0b50-244">d.</span></span> <span data-ttu-id="d0b50-245">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="d0b50-245">Click **Create**.</span></span>
 
### <a name="creating-a-hightail-test-user"></a><span data-ttu-id="d0b50-246">Hightail tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="d0b50-246">Creating a Hightail test user</span></span>

<span data-ttu-id="d0b50-247">hello ebben a szakaszban célja toocreate Hightail Britta Simon nevű felhasználó.</span><span class="sxs-lookup"><span data-stu-id="d0b50-247">hello objective of this section is toocreate a user called Britta Simon in Hightail.</span></span> 

<span data-ttu-id="d0b50-248">Nincs ebben a szakaszban az Ön művelet elem.</span><span class="sxs-lookup"><span data-stu-id="d0b50-248">There is no action item for you in this section.</span></span> <span data-ttu-id="d0b50-249">Hightail támogatja közvetlenül az idő a felhasználók átadása hello egyéni jogcímei alapján.</span><span class="sxs-lookup"><span data-stu-id="d0b50-249">Hightail supports just-in-time user provisioning based on hello custom claims.</span></span> <span data-ttu-id="d0b50-250">Ha már konfigurált egyéni jogcímek hello hello szakaszban látható  **[az Azure AD konfigurálása egyszeri bejelentkezéshez](#configuring-azure-ad-single-sign-on)**  újabb verziók esetén a felhasználók automatikusan létrehozása hello alkalmazás még nem létezik.</span><span class="sxs-lookup"><span data-stu-id="d0b50-250">If you have configured hello custom claims as shown in hello section **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** above, a user is automatically created in hello application it doesn't exist yet.</span></span> 

>[!NOTE]
><span data-ttu-id="d0b50-251">A felhasználó toocreate manuálisan kell, ha szüksége van-e toocontact hello [Hightail támogatási csoport](mailto:support@hightail.com).</span><span class="sxs-lookup"><span data-stu-id="d0b50-251">If you need toocreate a user manually, you need toocontact hello [Hightail support team](mailto:support@hightail.com).</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="d0b50-252">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="d0b50-252">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="d0b50-253">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooHightail megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="d0b50-253">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooHightail.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="d0b50-255">**tooassign Britta Simon tooHightail, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="d0b50-255">**tooassign Britta Simon tooHightail, perform hello following steps:**</span></span>

1. <span data-ttu-id="d0b50-256">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="d0b50-256">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="d0b50-258">Hello alkalmazások listában válassza ki a **Hightail**.</span><span class="sxs-lookup"><span data-stu-id="d0b50-258">In hello applications list, select **Hightail**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_app.png) 

3. <span data-ttu-id="d0b50-260">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="d0b50-260">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="d0b50-262">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="d0b50-262">Click **Add** button.</span></span> <span data-ttu-id="d0b50-263">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d0b50-263">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="d0b50-265">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="d0b50-265">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="d0b50-266">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d0b50-266">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="d0b50-267">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d0b50-267">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="d0b50-268">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="d0b50-268">Testing single sign-on</span></span>

<span data-ttu-id="d0b50-269">hello ebben a szakaszban célja tootest az egyszeri bejelentkezés konfigurációs használatával hello a hozzáférési Panel.</span><span class="sxs-lookup"><span data-stu-id="d0b50-269">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="d0b50-270">Ha a hozzáférési Panel hello hello Hightail csempe gombra kattint, automatikusan bejelentkezett tooyour Hightail alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="d0b50-270">When you click hello Hightail tile in hello Access Panel, you should get automatically signed-on tooyour Hightail application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="d0b50-271">További források</span><span class="sxs-lookup"><span data-stu-id="d0b50-271">Additional resources</span></span>

* [<span data-ttu-id="d0b50-272">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="d0b50-272">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="d0b50-273">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="d0b50-273">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_203.png

