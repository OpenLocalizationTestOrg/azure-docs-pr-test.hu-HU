---
title: "Oktatóanyag: Azure Active Directory-integráció Tableau kiszolgálóval |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a Tableau kiszolgáló között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c1917375-08aa-445c-a444-e22e23fa19e0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/18/2017
ms.author: jeedes
ms.openlocfilehash: feb2087bd6ae6ddcb920901e6719688fc95ae287
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-tableau-server"></a><span data-ttu-id="4c420-103">Oktatóanyag: Azure Active Directory-integráció Tableau kiszolgálóval</span><span class="sxs-lookup"><span data-stu-id="4c420-103">Tutorial: Azure Active Directory integration with Tableau Server</span></span>

<span data-ttu-id="4c420-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Tableau Server az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="4c420-104">In this tutorial, you learn how toointegrate Tableau Server with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="4c420-105">Tableau kiszolgáló integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="4c420-105">Integrating Tableau Server with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="4c420-106">Megadhatja a hozzáférés tooTableau Server rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="4c420-106">You can control in Azure AD who has access tooTableau Server</span></span>
- <span data-ttu-id="4c420-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooTableau (egyszeri bejelentkezés) kiszolgáló és az Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="4c420-107">You can enable your users tooautomatically get signed-on tooTableau Server (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="4c420-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="4c420-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="4c420-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="4c420-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4c420-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="4c420-110">Prerequisites</span></span>

<span data-ttu-id="4c420-111">tooconfigure Tableau Server az Azure AD integrálása, a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="4c420-111">tooconfigure Azure AD integration with Tableau Server, you need hello following items:</span></span>

- <span data-ttu-id="4c420-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="4c420-112">An Azure AD subscription</span></span>
- <span data-ttu-id="4c420-113">A Tableau Server egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="4c420-113">A Tableau Server single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="4c420-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="4c420-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="4c420-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="4c420-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="4c420-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="4c420-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="4c420-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="4c420-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="4c420-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="4c420-118">Scenario description</span></span>
<span data-ttu-id="4c420-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="4c420-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="4c420-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="4c420-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="4c420-121">Hello gyűjteményből Tableau kiszolgáló hozzáadása</span><span class="sxs-lookup"><span data-stu-id="4c420-121">Adding Tableau Server from hello gallery</span></span>
2. <span data-ttu-id="4c420-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="4c420-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-tableau-server-from-hello-gallery"></a><span data-ttu-id="4c420-123">Hello gyűjteményből Tableau kiszolgáló hozzáadása</span><span class="sxs-lookup"><span data-stu-id="4c420-123">Adding Tableau Server from hello gallery</span></span>
<span data-ttu-id="4c420-124">tooconfigure hello integrációs Tableau kiszolgáló, az Azure AD-be, meg kell tooadd Tableau Server hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="4c420-124">tooconfigure hello integration of Tableau Server into Azure AD, you need tooadd Tableau Server from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="4c420-125">**tooadd Tableau Server hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="4c420-125">**tooadd Tableau Server from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="4c420-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="4c420-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="4c420-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="4c420-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="4c420-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="4c420-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="4c420-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="4c420-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="4c420-133">Hello keresési mezőbe, írja be a **Tableau Server**.</span><span class="sxs-lookup"><span data-stu-id="4c420-133">In hello search box, type **Tableau Server**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_search.png)

5. <span data-ttu-id="4c420-135">A hello eredmények panelen válassza a **Tableau Server**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="4c420-135">In hello results panel, select **Tableau Server**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="4c420-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="4c420-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="4c420-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján Tableau kiszolgálóval</span><span class="sxs-lookup"><span data-stu-id="4c420-138">In this section, you configure and test Azure AD single sign-on with Tableau Server based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="4c420-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói Tableau Server tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="4c420-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Tableau Server is tooa user in Azure AD.</span></span> <span data-ttu-id="4c420-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználói hello Tableau Server közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="4c420-140">In other words, a link relationship between an Azure AD user and hello related user in Tableau Server needs toobe established.</span></span>

<span data-ttu-id="4c420-141">A Tableau kiszolgáló rendelje hello hello értékét **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="4c420-141">In Tableau Server, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="4c420-142">tooconfigure és az Azure AD az egyszeri bejelentkezés tesztelése Tableau kiszolgálóval, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="4c420-142">tooconfigure and test Azure AD single sign-on with Tableau Server, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="4c420-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="4c420-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="4c420-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="4c420-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="4c420-145">**[A Tableau Server tesztfelhasználó létrehozása](#creating-a-tableau-server-test-user)**  -toohave egy megfelelője a Britta Simon Tableau Server csatolt toohello az Azure AD felhasználói ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="4c420-145">**[Creating a Tableau Server test user](#creating-a-tableau-server-test-user)** - toohave a counterpart of Britta Simon in Tableau Server that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="4c420-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="4c420-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="4c420-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="4c420-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="4c420-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="4c420-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="4c420-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és a Tableau kiszolgálóalkalmazás az egyszeri bejelentkezés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="4c420-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Tableau Server application.</span></span>

<span data-ttu-id="4c420-150">**az Azure AD tooconfigure egyszeri bejelentkezés Tableau kiszolgálóval, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="4c420-150">**tooconfigure Azure AD single sign-on with Tableau Server, perform hello following steps:**</span></span>

1. <span data-ttu-id="4c420-151">Az Azure portál, a hello hello **Tableau Server** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="4c420-151">In hello Azure portal, on hello **Tableau Server** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="4c420-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="4c420-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_samlbase.png)

3. <span data-ttu-id="4c420-155">A hello **Tableau kiszolgáló tartományával és URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="4c420-155">On hello **Tableau Server Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_url.png)

    <span data-ttu-id="4c420-157">a.</span><span class="sxs-lookup"><span data-stu-id="4c420-157">a.</span></span> <span data-ttu-id="4c420-158">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://azure.<domain name>.link`</span><span class="sxs-lookup"><span data-stu-id="4c420-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://azure.<domain name>.link`</span></span>
    
    <span data-ttu-id="4c420-159">b.</span><span class="sxs-lookup"><span data-stu-id="4c420-159">b.</span></span> <span data-ttu-id="4c420-160">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://azure.<domain name>.link`</span><span class="sxs-lookup"><span data-stu-id="4c420-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://azure.<domain name>.link`</span></span>

    <span data-ttu-id="4c420-161">c.</span><span class="sxs-lookup"><span data-stu-id="4c420-161">c.</span></span> <span data-ttu-id="4c420-162">A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://azure.<domain name>.link/wg/saml/SSO/index.html`</span><span class="sxs-lookup"><span data-stu-id="4c420-162">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://azure.<domain name>.link/wg/saml/SSO/index.html`</span></span>
     
    > [!NOTE] 
    > <span data-ttu-id="4c420-163">hello előző értékek nem valódi értékek.</span><span class="sxs-lookup"><span data-stu-id="4c420-163">hello preceding values are not real values.</span></span> <span data-ttu-id="4c420-164">Később frissítse a hello értékeket hello tényleges URL-cím és az hello Tableau kiszolgáló konfigurációs lapon azonosítója.</span><span class="sxs-lookup"><span data-stu-id="4c420-164">Later, you update hello values with hello actual URL and identifier from hello Tableau Server configuration page.</span></span> 

4. <span data-ttu-id="4c420-165">Tableau kiszolgálóalkalmazás hello SAML helyességi feltételek vár egy meghatározott formátumban.</span><span class="sxs-lookup"><span data-stu-id="4c420-165">Tableau Server application expects hello SAML assertions in a specific format.</span></span> <span data-ttu-id="4c420-166">Az alkalmazás jogcímek a következő hello konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="4c420-166">Configure hello following claims for this application.</span></span> <span data-ttu-id="4c420-167">Ezek az attribútumok értékének hello kezelheti hello **"Felhasználói attribútumok"** szakasz alkalmazás integráció lapján.</span><span class="sxs-lookup"><span data-stu-id="4c420-167">You can manage hello values of these attributes from hello **"User Attributes"** section on application integration page.</span></span> <span data-ttu-id="4c420-168">hello alábbi képernyőfelvételen szereplő példán látható a hello azonos.</span><span class="sxs-lookup"><span data-stu-id="4c420-168">hello following screenshot shows an example for hello same.</span></span>
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-tableauserver-tutorial/3.png)
    
5. <span data-ttu-id="4c420-170">A hello **felhasználói attribútumok** hello című szakaszban **egyszeri bejelentkezés** párbeszédpanelen konfigurálja a SAML-jogkivonat attribútum fenti hello ábrán látható módon, és hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="4c420-170">In hello **User Attributes** section on hello **Single sign-on** dialog, configure SAML token attribute as shown in hello image above and perform hello following steps:</span></span>
    
    | <span data-ttu-id="4c420-171">Attribútum neve</span><span class="sxs-lookup"><span data-stu-id="4c420-171">Attribute Name</span></span> | <span data-ttu-id="4c420-172">Attribútum-érték</span><span class="sxs-lookup"><span data-stu-id="4c420-172">Attribute Value</span></span> |
    | ---------------| --------------- |    
    | <span data-ttu-id="4c420-173">felhasználónév</span><span class="sxs-lookup"><span data-stu-id="4c420-173">username</span></span> | <span data-ttu-id="4c420-174">*User.DisplayName*</span><span class="sxs-lookup"><span data-stu-id="4c420-174">*user.displayname*</span></span> |

    <span data-ttu-id="4c420-175">a.</span><span class="sxs-lookup"><span data-stu-id="4c420-175">a.</span></span> <span data-ttu-id="4c420-176">Kattintson a **Hozzáadás attribútum** tooopen hello **attribútum hozzáadása** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="4c420-176">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-tableauserver-tutorial/tutorial_officespace_04.png)

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-tableauserver-tutorial/tutorial_officespace_05.png)
    
    <span data-ttu-id="4c420-179">b.</span><span class="sxs-lookup"><span data-stu-id="4c420-179">b.</span></span> <span data-ttu-id="4c420-180">A hello **neve** szövegmezőben, az adott sorhoz feltüntetett hello attribútum neve.</span><span class="sxs-lookup"><span data-stu-id="4c420-180">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>
    
    <span data-ttu-id="4c420-181">c.</span><span class="sxs-lookup"><span data-stu-id="4c420-181">c.</span></span> <span data-ttu-id="4c420-182">A hello **érték** listájában, hello attribútuma Típusérték az adott sorhoz feltüntetett.</span><span class="sxs-lookup"><span data-stu-id="4c420-182">From hello **Value** list, type hello attribute value shown for that row.</span></span>
    
    <span data-ttu-id="4c420-183">d.</span><span class="sxs-lookup"><span data-stu-id="4c420-183">d.</span></span> <span data-ttu-id="4c420-184">Kattintson a **Ok**</span><span class="sxs-lookup"><span data-stu-id="4c420-184">Click **Ok**</span></span>


6. <span data-ttu-id="4c420-185">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="4c420-185">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_certificate.png) 

7. <span data-ttu-id="4c420-187">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="4c420-187">Click **Save** button.</span></span>

    <span data-ttu-id="4c420-188">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-tableauserver-tutorial/tutorial_general_400.png)
<CS></span><span class="sxs-lookup"><span data-stu-id="4c420-188">![Configure Single Sign-On](./media/active-directory-saas-tableauserver-tutorial/tutorial_general_400.png)
<CS></span></span>
8. <span data-ttu-id="4c420-189">tooget az alkalmazáshoz konfigurált SSO, toosign tooyour Tableau Server bérlői rendszergazdaként kell.</span><span class="sxs-lookup"><span data-stu-id="4c420-189">tooget SSO configured for your application, you need toosign-on tooyour Tableau Server tenant as an administrator.</span></span>
   
   <span data-ttu-id="4c420-190">a.</span><span class="sxs-lookup"><span data-stu-id="4c420-190">a.</span></span> <span data-ttu-id="4c420-191">A hello Tableau kiszolgálókonfiguráció lapon kattintson a hello **SAML** fülre.</span><span class="sxs-lookup"><span data-stu-id="4c420-191">In hello Tableau Server configuration, click hello **SAML** tab.</span></span>
  
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_001.png) 
  
   <span data-ttu-id="4c420-193">b.</span><span class="sxs-lookup"><span data-stu-id="4c420-193">b.</span></span> <span data-ttu-id="4c420-194">Válassza ki a hello jelölőnégyzetet az **az egyszeri bejelentkezéshez használható SAML**.</span><span class="sxs-lookup"><span data-stu-id="4c420-194">Select hello checkbox of **Use SAML for single sign-on**.</span></span>
   
   <span data-ttu-id="4c420-195">c.</span><span class="sxs-lookup"><span data-stu-id="4c420-195">c.</span></span> <span data-ttu-id="4c420-196">Tableau kiszolgálói válasz URL-cím – hello Tableau Server felhasználók hozzáférhetnek, például a http://tableau_server URL-címet.</span><span class="sxs-lookup"><span data-stu-id="4c420-196">Tableau Server return URL—hello URL that Tableau Server users will be accessing, such as http://tableau_server.</span></span> <span data-ttu-id="4c420-197">Http://localhost használata nem ajánlott.</span><span class="sxs-lookup"><span data-stu-id="4c420-197">Using http://localhost is not recommended.</span></span> <span data-ttu-id="4c420-198">Egy URL-címet a záró perjelet (például http://tableau_server/) használata nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="4c420-198">Using a URL with a trailing slash (for example, http://tableau_server/) is not supported.</span></span> <span data-ttu-id="4c420-199">Másolás **Tableau kiszolgálói válasz URL-cím** és illessze be tooAzure AD **URL-cím bejelentkezési** textbox **Tableau kiszolgáló tartományával és URL-címek** szakasz.</span><span class="sxs-lookup"><span data-stu-id="4c420-199">Copy **Tableau Server return URL** and paste it tooAzure AD **Sign On URL** textbox in **Tableau Server Domain and URLs** section.</span></span>
   
   <span data-ttu-id="4c420-200">d.</span><span class="sxs-lookup"><span data-stu-id="4c420-200">d.</span></span> <span data-ttu-id="4c420-201">SAML Entitásazonosító – hello Entitásazonosító egyedileg azonosítja a Tableau kiszolgáló telepítési toohello IdP.</span><span class="sxs-lookup"><span data-stu-id="4c420-201">SAML entity ID—hello entity ID uniquely identifies your Tableau Server installation toohello IdP.</span></span> <span data-ttu-id="4c420-202">Adhatja meg a Tableau URL-címe újra ide, ha szeretné, de nem rendelkezik toobe a Tableau URL-címe.</span><span class="sxs-lookup"><span data-stu-id="4c420-202">You can enter your Tableau Server URL again here, if you like, but it does not have toobe your Tableau Server URL.</span></span> <span data-ttu-id="4c420-203">Másolás **SAML Entitásazonosító** és illessze be tooAzure AD **azonosító** textbox **Tableau kiszolgáló tartományával és URL-címek** szakasz.</span><span class="sxs-lookup"><span data-stu-id="4c420-203">Copy **SAML entity ID** and paste it tooAzure AD **Identifier** textbox in **Tableau Server Domain and URLs** section.</span></span>
     
   <span data-ttu-id="4c420-204">e.</span><span class="sxs-lookup"><span data-stu-id="4c420-204">e.</span></span> <span data-ttu-id="4c420-205">Kattintson a hello **metaadat-fájl exportálása** hello text editor alkalmazásban nyissa meg.</span><span class="sxs-lookup"><span data-stu-id="4c420-205">Click hello **Export Metadata File** and open it in hello text editor application.</span></span> <span data-ttu-id="4c420-206">Keresse meg a helyességi feltétel ügyfél szolgáltatás URL-címe a Http Post és Index 0 és hello URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="4c420-206">Locate Assertion Consumer Service URL with Http Post and Index 0 and copy hello URL.</span></span> <span data-ttu-id="4c420-207">Most illessze tooAzure AD **válasz URL-CÍMEN** textbox **Tableau kiszolgáló tartományával és URL-címek** szakasz.</span><span class="sxs-lookup"><span data-stu-id="4c420-207">Now paste it tooAzure AD **Reply URL** textbox in **Tableau Server Domain and URLs** section.</span></span>
   
   <span data-ttu-id="4c420-208">f.</span><span class="sxs-lookup"><span data-stu-id="4c420-208">f.</span></span> <span data-ttu-id="4c420-209">Keresse meg az összevonási metaadatok fájlt az Azure portálról letöltött, majd töltse fel az hello **SAML Idp metaadatfájl**.</span><span class="sxs-lookup"><span data-stu-id="4c420-209">Locate your Federation Metadata file downloaded from Azure portal, and then upload it in hello **SAML Idp metadata file**.</span></span>
   
   <span data-ttu-id="4c420-210">g.</span><span class="sxs-lookup"><span data-stu-id="4c420-210">g.</span></span> <span data-ttu-id="4c420-211">Kattintson a hello **OK** hello Tableau kiszolgálókonfiguráció lapon gombra.</span><span class="sxs-lookup"><span data-stu-id="4c420-211">Click hello **OK** button in hello Tableau Server Configuration page.</span></span>
   
    >[!NOTE] 
    ><span data-ttu-id="4c420-212">Vevő minden tanúsítvány rendelkezik tooupload hello Tableau kiszolgáló SAML SSO konfigurációs, és figyelmen beolvasása hello egyszeri bejelentkezési folyamata.</span><span class="sxs-lookup"><span data-stu-id="4c420-212">Customer have tooupload any certificate in hello Tableau Server SAML SSO configuration and it will get ignored in hello SSO flow.</span></span>
    ><span data-ttu-id="4c420-213">Ha kell súgó a SAML konfigurálása a Tableau kiszolgálón, akkor tekintse meg a toothis cikk [SAML konfigurálása](http://onlinehelp.tableau.com/current/server/en-us/config_saml.htm).</span><span class="sxs-lookup"><span data-stu-id="4c420-213">If you need help configuring SAML on Tableau Server then please refer toothis article [Configure SAML](http://onlinehelp.tableau.com/current/server/en-us/config_saml.htm).</span></span>
    >
<CE>

> [!TIP]
> <span data-ttu-id="4c420-214">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="4c420-214">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="4c420-215">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="4c420-215">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="4c420-216">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="4c420-216">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="4c420-217">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="4c420-217">Creating an Azure AD test user</span></span>
<span data-ttu-id="4c420-218">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="4c420-218">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="4c420-220">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="4c420-220">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="4c420-221">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="4c420-221">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="4c420-223">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="4c420-223">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="4c420-225">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="4c420-225">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="4c420-227">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="4c420-227">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="4c420-229">a.</span><span class="sxs-lookup"><span data-stu-id="4c420-229">a.</span></span> <span data-ttu-id="4c420-230">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="4c420-230">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="4c420-231">b.</span><span class="sxs-lookup"><span data-stu-id="4c420-231">b.</span></span> <span data-ttu-id="4c420-232">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="4c420-232">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="4c420-233">c.</span><span class="sxs-lookup"><span data-stu-id="4c420-233">c.</span></span> <span data-ttu-id="4c420-234">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="4c420-234">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="4c420-235">d.</span><span class="sxs-lookup"><span data-stu-id="4c420-235">d.</span></span> <span data-ttu-id="4c420-236">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="4c420-236">Click **Create**.</span></span>
 
### <a name="creating-a-tableau-server-test-user"></a><span data-ttu-id="4c420-237">A Tableau Server tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="4c420-237">Creating a Tableau Server test user</span></span>

<span data-ttu-id="4c420-238">hello ebben a szakaszban célja toocreate Tableau Server Britta Simon nevű felhasználó.</span><span class="sxs-lookup"><span data-stu-id="4c420-238">hello objective of this section is toocreate a user called Britta Simon in Tableau Server.</span></span> <span data-ttu-id="4c420-239">Tooprovision hello Tableau Server senki hello kell.</span><span class="sxs-lookup"><span data-stu-id="4c420-239">You need tooprovision all hello users in hello Tableau server.</span></span> 

<span data-ttu-id="4c420-240">Adott felhasználó felhasználóneve, hello meg kell felelnie a hello érték, amely egyéni attribútumának hello Azure AD-be vannak állítva **felhasználónév**.</span><span class="sxs-lookup"><span data-stu-id="4c420-240">That username of hello user should match hello value which you have configured in hello Azure AD custom attribute of **username**.</span></span> <span data-ttu-id="4c420-241">A megfelelő hello integrációs leképezési kell működnie hello [az Azure AD konfigurálása egyszeri bejelentkezéshez](#configuring-azure-ad-single-sign-on).</span><span class="sxs-lookup"><span data-stu-id="4c420-241">With hello correct mapping hello integration should work [Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on).</span></span>

>[!NOTE]
><span data-ttu-id="4c420-242">Ha egy felhasználó toocreate manuálisan kell, a szervezet kell toocontact hello Tableau kiszolgáló rendszergazdájához.</span><span class="sxs-lookup"><span data-stu-id="4c420-242">If you need toocreate a user manually, you need toocontact hello Tableau Server administrator in your organization.</span></span>
> 
> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="4c420-243">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="4c420-243">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="4c420-244">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooTableau kiszolgáló megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="4c420-244">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooTableau Server.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="4c420-246">**tooassign Britta Simon tooTableau kiszolgáló, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="4c420-246">**tooassign Britta Simon tooTableau Server, perform hello following steps:**</span></span>

1. <span data-ttu-id="4c420-247">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="4c420-247">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="4c420-249">Hello alkalmazások listában válassza ki a **Tableau Server**.</span><span class="sxs-lookup"><span data-stu-id="4c420-249">In hello applications list, select **Tableau Server**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_app.png) 

3. <span data-ttu-id="4c420-251">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="4c420-251">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="4c420-253">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="4c420-253">Click **Add** button.</span></span> <span data-ttu-id="4c420-254">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="4c420-254">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="4c420-256">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="4c420-256">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="4c420-257">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="4c420-257">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="4c420-258">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="4c420-258">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="4c420-259">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="4c420-259">Testing single sign-on</span></span>

<span data-ttu-id="4c420-260">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="4c420-260">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="4c420-261">Ha a hozzáférési Panel hello hello Tableau Server csempe gombra kattint, automatikusan bejelentkezett tooyour Tableau kiszolgálóalkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="4c420-261">When you click hello Tableau Server tile in hello Access Panel, you should get automatically signed-on tooyour Tableau Server application.</span></span>
<span data-ttu-id="4c420-262">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="4c420-262">For more information about the Access Panel, see [introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="4c420-263">További források</span><span class="sxs-lookup"><span data-stu-id="4c420-263">Additional resources</span></span>

* [<span data-ttu-id="4c420-264">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="4c420-264">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="4c420-265">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="4c420-265">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_203.png

