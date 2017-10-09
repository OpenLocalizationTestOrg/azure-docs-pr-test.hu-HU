---
title: "Oktatóanyag: Azure Active Directoryval integrált Learningpool Act |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a törvény Learningpool között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 51e8695f-31e1-4d09-8eb3-13241999d99f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes
ms.openlocfilehash: f343623f08bb60e143aaff07d93e4ef773232e07
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-learningpool-act"></a><span data-ttu-id="1bd24-103">Oktatóanyag: Azure Active Directoryval integrált Learningpool Act</span><span class="sxs-lookup"><span data-stu-id="1bd24-103">Tutorial: Azure Active Directory integration with Learningpool Act</span></span>

<span data-ttu-id="1bd24-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Learningpool működni az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="1bd24-104">In this tutorial, you learn how toointegrate Learningpool Act with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="1bd24-105">Learningpool Act integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="1bd24-105">Integrating Learningpool Act with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="1bd24-106">Megadhatja a hozzáférés tooLearningpool Act rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="1bd24-106">You can control in Azure AD who has access tooLearningpool Act</span></span>
- <span data-ttu-id="1bd24-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooLearningpool Act (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="1bd24-107">You can enable your users tooautomatically get signed-on tooLearningpool Act (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="1bd24-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="1bd24-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="1bd24-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="1bd24-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1bd24-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="1bd24-110">Prerequisites</span></span>

<span data-ttu-id="1bd24-111">az Azure AD integrálása Learningpool Act tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="1bd24-111">tooconfigure Azure AD integration with Learningpool Act, you need hello following items:</span></span>

- <span data-ttu-id="1bd24-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="1bd24-112">An Azure AD subscription</span></span>
- <span data-ttu-id="1bd24-113">Egy Learningpool Act egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="1bd24-113">A Learningpool Act single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="1bd24-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="1bd24-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="1bd24-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="1bd24-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="1bd24-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="1bd24-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="1bd24-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="1bd24-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="1bd24-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="1bd24-118">Scenario description</span></span>
<span data-ttu-id="1bd24-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="1bd24-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="1bd24-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="1bd24-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="1bd24-121">Hello gyűjteményből Learningpool Act hozzáadása</span><span class="sxs-lookup"><span data-stu-id="1bd24-121">Adding Learningpool Act from hello gallery</span></span>
2. <span data-ttu-id="1bd24-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="1bd24-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-learningpool-act-from-hello-gallery"></a><span data-ttu-id="1bd24-123">Hello gyűjteményből Learningpool Act hozzáadása</span><span class="sxs-lookup"><span data-stu-id="1bd24-123">Adding Learningpool Act from hello gallery</span></span>
<span data-ttu-id="1bd24-124">tooconfigure hello integrációs Learningpool törvény, az Azure AD-be, meg kell tooadd Learningpool Act hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="1bd24-124">tooconfigure hello integration of Learningpool Act into Azure AD, you need tooadd Learningpool Act from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="1bd24-125">**tooadd Learningpool Act hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="1bd24-125">**tooadd Learningpool Act from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="1bd24-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="1bd24-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="1bd24-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="1bd24-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="1bd24-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="1bd24-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="1bd24-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="1bd24-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="1bd24-133">Hello keresési mezőbe, írja be a **Learningpool Act**.</span><span class="sxs-lookup"><span data-stu-id="1bd24-133">In hello search box, type **Learningpool Act**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-Learningpool-tutorial/tutorial_Learningpoolact_search.png)

5. <span data-ttu-id="1bd24-135">A hello eredmények panelen válassza ki a **Learningpool Act**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="1bd24-135">In hello results panel, select **Learningpool Act**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-Learningpool-tutorial/tutorial_Learningpoolact_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="1bd24-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="1bd24-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="1bd24-138">Ebben a szakaszban konfigurálhatja, és az Azure AD az egyszeri bejelentkezés Learningpool Act-teszthez "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="1bd24-138">In this section, you configure and test Azure AD single sign-on with Learningpool Act based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="1bd24-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Learningpool Act tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="1bd24-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Learningpool Act is tooa user in Azure AD.</span></span> <span data-ttu-id="1bd24-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Learningpool Act közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="1bd24-140">In other words, a link relationship between an Azure AD user and hello related user in Learningpool Act needs toobe established.</span></span>

<span data-ttu-id="1bd24-141">Learningpool Act, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="1bd24-141">In Learningpool Act, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="1bd24-142">tooconfigure és az Azure AD az egyszeri bejelentkezés Learningpool Act-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="1bd24-142">tooconfigure and test Azure AD single sign-on with Learningpool Act, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="1bd24-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="1bd24-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="1bd24-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="1bd24-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="1bd24-145">**[Learningpool Act tesztfelhasználó létrehozása](#creating-a-learningpool-act-test-user)**  -toohave egy megfelelője a Britta Simon a Learningpool Act, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="1bd24-145">**[Creating a Learningpool Act test user](#creating-a-learningpool-act-test-user)** - toohave a counterpart of Britta Simon in Learningpool Act that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="1bd24-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="1bd24-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="1bd24-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="1bd24-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="1bd24-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="1bd24-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="1bd24-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Learningpool Act-alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="1bd24-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Learningpool Act application.</span></span>

<span data-ttu-id="1bd24-150">**tooconfigure az Azure AD egyszeri bejelentkezést a Learningpool Act, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="1bd24-150">**tooconfigure Azure AD single sign-on with Learningpool Act, perform hello following steps:**</span></span>

1. <span data-ttu-id="1bd24-151">Az Azure portál, a hello hello **Learningpool Act** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="1bd24-151">In hello Azure portal, on hello **Learningpool Act** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="1bd24-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="1bd24-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-Learningpool-tutorial/tutorial_Learningpoolact_samlbase.png)

3. <span data-ttu-id="1bd24-155">A hello **Learningpool Act tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="1bd24-155">On hello **Learningpool Act Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-Learningpool-tutorial/tutorial_Learningpoolact_url.png)

    <span data-ttu-id="1bd24-157">a.</span><span class="sxs-lookup"><span data-stu-id="1bd24-157">a.</span></span> <span data-ttu-id="1bd24-158">A hello **bejelentkezési URL-cím** szövegmezőhöz típus hello URL-címe:`https://parliament.preview.Learningpool.com/auth/shibboleth/index.php`</span><span class="sxs-lookup"><span data-stu-id="1bd24-158">In hello **Sign-on URL** textbox, type hello URL: `https://parliament.preview.Learningpool.com/auth/shibboleth/index.php`</span></span>

    <span data-ttu-id="1bd24-159">b.</span><span class="sxs-lookup"><span data-stu-id="1bd24-159">b.</span></span> <span data-ttu-id="1bd24-160">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:</span><span class="sxs-lookup"><span data-stu-id="1bd24-160">In hello **Identifier** textbox, type a URL using hello following pattern:</span></span>
    | |
    |--|
    | `https://<subdomain>.Learningpool.com/shibboleth` |
    | `https://<subdomain>.preview.Learningpool.com/shibboleth` |

    > [!NOTE] 
    > <span data-ttu-id="1bd24-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="1bd24-161">These values are not real.</span></span> <span data-ttu-id="1bd24-162">Frissítse a bejelentkezési URL-cím és azonosító a hello tényleges értékek.</span><span class="sxs-lookup"><span data-stu-id="1bd24-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="1bd24-163">Ügyfél [Learningpool Act ügyfél-támogatási csoport](https://www.Learningpool.com/support) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="1bd24-163">Contact [Learningpool Act Client support team](https://www.Learningpool.com/support) tooget these values.</span></span> 
 
4. <span data-ttu-id="1bd24-164">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="1bd24-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-Learningpool-tutorial/tutorial_Learningpoolact_certificate.png) 

5. <span data-ttu-id="1bd24-166">Learningpool Act alkalmazás hello SAML helyességi feltételek vár egy meghatározott formátumban.</span><span class="sxs-lookup"><span data-stu-id="1bd24-166">Learningpool Act application expects hello SAML assertions in a specific format.</span></span> <span data-ttu-id="1bd24-167">Állítsa be az alkalmazás jogcímek a következő hello.</span><span class="sxs-lookup"><span data-stu-id="1bd24-167">Please configure hello following claims for this application.</span></span> <span data-ttu-id="1bd24-168">Ezek az attribútumok értékének hello kezelheti hello **"Atrribute"** hello alkalmazás lapján.</span><span class="sxs-lookup"><span data-stu-id="1bd24-168">You can manage hello values of these attributes from hello **"Atrribute"** tab of hello application.</span></span> <span data-ttu-id="1bd24-169">a következő képernyőkép hello ezen mutat egy példát.</span><span class="sxs-lookup"><span data-stu-id="1bd24-169">hello following screenshot shows an example for this.</span></span> 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-Learningpool-tutorial/tutorial_Learningpoolact_attribute.png) 

6. <span data-ttu-id="1bd24-171">A hello **felhasználói attribútumok** szakaszt, hello **egyszeri bejelentkezés** párbeszédpanelen konfigurálja a SAML-jogkivonat attribútum hello ábrának megfelelően, és hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="1bd24-171">In hello **User Attributes** section on hello **Single sign-on** dialog, configure SAML token attribute as shown in hello image and perform hello following steps:</span></span>
    
    | <span data-ttu-id="1bd24-172">Attribútum neve</span><span class="sxs-lookup"><span data-stu-id="1bd24-172">Attribute Name</span></span> | <span data-ttu-id="1bd24-173">Attribútum-érték</span><span class="sxs-lookup"><span data-stu-id="1bd24-173">Attribute Value</span></span> |
    | ------------------- | -------------------- |
    | <span data-ttu-id="1bd24-174">urn: oid:1.2.840.113556.1.4.221</span><span class="sxs-lookup"><span data-stu-id="1bd24-174">urn:oid:1.2.840.113556.1.4.221</span></span> | <span data-ttu-id="1bd24-175">User.userPrincipalName</span><span class="sxs-lookup"><span data-stu-id="1bd24-175">user.userprincipalname</span></span> |
    | <span data-ttu-id="1bd24-176">urn: oid:2.5.4.42</span><span class="sxs-lookup"><span data-stu-id="1bd24-176">urn:oid:2.5.4.42</span></span> | <span data-ttu-id="1bd24-177">User.givenName</span><span class="sxs-lookup"><span data-stu-id="1bd24-177">user.givenname</span></span> |
    | <span data-ttu-id="1bd24-178">urn: oid:0.9.2342.19200300.100.1.3</span><span class="sxs-lookup"><span data-stu-id="1bd24-178">urn:oid:0.9.2342.19200300.100.1.3</span></span> | <span data-ttu-id="1bd24-179">User.mail</span><span class="sxs-lookup"><span data-stu-id="1bd24-179">user.mail</span></span> |    
    | <span data-ttu-id="1bd24-180">urn: oid:2.5.4.4</span><span class="sxs-lookup"><span data-stu-id="1bd24-180">urn:oid:2.5.4.4</span></span> | <span data-ttu-id="1bd24-181">User.surname</span><span class="sxs-lookup"><span data-stu-id="1bd24-181">user.surname</span></span> |
    
    <span data-ttu-id="1bd24-182">a.</span><span class="sxs-lookup"><span data-stu-id="1bd24-182">a.</span></span> <span data-ttu-id="1bd24-183">Kattintson a **Hozzáadás attribútum** tooopen hello **attribútum hozzáadása** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="1bd24-183">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-Learningpool-tutorial/tutorial_attribute_04.png)

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-Learningpool-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="1bd24-186">b.</span><span class="sxs-lookup"><span data-stu-id="1bd24-186">b.</span></span> <span data-ttu-id="1bd24-187">A hello **neve** szövegmezőben, az adott sorhoz feltüntetett hello attribútum neve.</span><span class="sxs-lookup"><span data-stu-id="1bd24-187">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>

    <span data-ttu-id="1bd24-188">c.</span><span class="sxs-lookup"><span data-stu-id="1bd24-188">c.</span></span> <span data-ttu-id="1bd24-189">A hello **érték** listájában, hello attribútuma Típusérték az adott sorhoz feltüntetett.</span><span class="sxs-lookup"><span data-stu-id="1bd24-189">From hello **Value** list, type hello attribute value shown for that row.</span></span>

    <span data-ttu-id="1bd24-190">d.</span><span class="sxs-lookup"><span data-stu-id="1bd24-190">d.</span></span> <span data-ttu-id="1bd24-191">Hagyja hello **Namespace** üres.</span><span class="sxs-lookup"><span data-stu-id="1bd24-191">Leave hello **Namespace** blank.</span></span>
    
    <span data-ttu-id="1bd24-192">e.</span><span class="sxs-lookup"><span data-stu-id="1bd24-192">e.</span></span> <span data-ttu-id="1bd24-193">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="1bd24-193">Click **Ok**.</span></span>

7. <span data-ttu-id="1bd24-194">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="1bd24-194">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-Learningpool-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="1bd24-196">tooconfigure egyszeri bejelentkezést a **Learningpool Act** oldalon kell letöltött toosend hello **metaadatainak XML-kódja** túl[Learningpool Act támogatási csoport](https://www.Learningpool.com/support).</span><span class="sxs-lookup"><span data-stu-id="1bd24-196">tooconfigure single sign-on on **Learningpool Act** side, you need toosend hello downloaded **Metadata XML** too[Learningpool Act support team](https://www.Learningpool.com/support).</span></span> <span data-ttu-id="1bd24-197">Maguk állítják be ezt a beállítást toohave hello SAML SSO kapcsolat mindkét oldalán megfelelően beállítva.</span><span class="sxs-lookup"><span data-stu-id="1bd24-197">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="1bd24-198">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="1bd24-198">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="1bd24-199">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="1bd24-199">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="1bd24-200">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="1bd24-200">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="1bd24-201">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="1bd24-201">Creating an Azure AD test user</span></span>
<span data-ttu-id="1bd24-202">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="1bd24-202">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="1bd24-204">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="1bd24-204">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="1bd24-205">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="1bd24-205">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-Learningpool-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="1bd24-207">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="1bd24-207">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-Learningpool-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="1bd24-209">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="1bd24-209">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-Learningpool-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="1bd24-211">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="1bd24-211">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-Learningpool-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="1bd24-213">a.</span><span class="sxs-lookup"><span data-stu-id="1bd24-213">a.</span></span> <span data-ttu-id="1bd24-214">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="1bd24-214">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="1bd24-215">b.</span><span class="sxs-lookup"><span data-stu-id="1bd24-215">b.</span></span> <span data-ttu-id="1bd24-216">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="1bd24-216">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="1bd24-217">c.</span><span class="sxs-lookup"><span data-stu-id="1bd24-217">c.</span></span> <span data-ttu-id="1bd24-218">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="1bd24-218">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="1bd24-219">d.</span><span class="sxs-lookup"><span data-stu-id="1bd24-219">d.</span></span> <span data-ttu-id="1bd24-220">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="1bd24-220">Click **Create**.</span></span>
 
### <a name="creating-a-learningpool-act-test-user"></a><span data-ttu-id="1bd24-221">Learningpool Act tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="1bd24-221">Creating a Learningpool Act test user</span></span>

<span data-ttu-id="1bd24-222">az Azure AD tooenable felhasználók toolog a tooLearningpool Act, azok ki kell építenie Learningpool Act be.</span><span class="sxs-lookup"><span data-stu-id="1bd24-222">tooenable Azure AD users toolog in tooLearningpool Act, they must be provisioned into Learningpool Act.</span></span>

<span data-ttu-id="1bd24-223">Nincs a akkor tooconfigure felhasználók átadásához tooLearningpool Act művelet elem.</span><span class="sxs-lookup"><span data-stu-id="1bd24-223">There is no action item for you tooconfigure user provisioning tooLearningpool Act.</span></span>  
<span data-ttu-id="1bd24-224">A felhasználóknak kell toobe hozta létre a [Learningpool Act támogatási csoport](https://www.Learningpool.com/support).</span><span class="sxs-lookup"><span data-stu-id="1bd24-224">Users need toobe created by your [Learningpool Act support team](https://www.Learningpool.com/support).</span></span>

>[!NOTE]
><span data-ttu-id="1bd24-225">Bármely más Learningpool Act felhasználói fiók létrehozása eszközök vagy Learningpool Act tooprovision által nyújtott API-k AAD felhasználói fiókokat.</span><span class="sxs-lookup"><span data-stu-id="1bd24-225">You can use any other Learningpool Act user account creation tools or APIs provided by Learningpool Act tooprovision AAD user accounts.</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="1bd24-226">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="1bd24-226">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="1bd24-227">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooLearningpool Act megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="1bd24-227">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooLearningpool Act.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="1bd24-229">**tooassign Britta Simon tooLearningpool Act, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="1bd24-229">**tooassign Britta Simon tooLearningpool Act, perform hello following steps:**</span></span>

1. <span data-ttu-id="1bd24-230">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="1bd24-230">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="1bd24-232">Hello alkalmazások listában válassza ki a **Learningpool Act**.</span><span class="sxs-lookup"><span data-stu-id="1bd24-232">In hello applications list, select **Learningpool Act**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-Learningpool-tutorial/tutorial_Learningpoolact_app.png) 

3. <span data-ttu-id="1bd24-234">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="1bd24-234">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="1bd24-236">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="1bd24-236">Click **Add** button.</span></span> <span data-ttu-id="1bd24-237">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="1bd24-237">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="1bd24-239">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="1bd24-239">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="1bd24-240">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="1bd24-240">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="1bd24-241">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="1bd24-241">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="1bd24-242">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="1bd24-242">Testing single sign-on</span></span>

<span data-ttu-id="1bd24-243">hello ebben a szakaszban célja tootest az egyszeri bejelentkezés konfigurációs használatával hello a hozzáférési Panel.</span><span class="sxs-lookup"><span data-stu-id="1bd24-243">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="1bd24-244">Hello Learningpool Act hello hozzáférési Panel csempére kattintva kapja meg automatikusan bejelentkezett tooyour Learningpool Act alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="1bd24-244">When you click hello Learningpool Act tile in hello Access Panel, you should get automatically signed-on tooyour Learningpool Act application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1bd24-245">További források</span><span class="sxs-lookup"><span data-stu-id="1bd24-245">Additional resources</span></span>

* [<span data-ttu-id="1bd24-246">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="1bd24-246">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="1bd24-247">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="1bd24-247">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-Learningpool-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-Learningpool-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-Learningpool-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-Learningpool-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-Learningpool-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-Learningpool-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-Learningpool-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-Learningpool-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-Learningpool-tutorial/tutorial_general_203.png

