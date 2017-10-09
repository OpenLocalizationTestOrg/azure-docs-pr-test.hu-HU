---
title: "Oktatóanyag: Azure Active Directoryval integrált Evernote |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Evernote között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 28acce3e-22a0-4a37-8b66-6e518d777350
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.openlocfilehash: 4d7017e571ed12a0b155aa188c6b0ecb3c9898a2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-evernote"></a><span data-ttu-id="87e0a-103">Oktatóanyag: Azure Active Directoryval integrált Evernote</span><span class="sxs-lookup"><span data-stu-id="87e0a-103">Tutorial: Azure Active Directory integration with Evernote</span></span>

<span data-ttu-id="87e0a-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Evernote az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="87e0a-104">In this tutorial, you learn how toointegrate Evernote with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="87e0a-105">Evernote integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="87e0a-105">Integrating Evernote with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="87e0a-106">Az Azure AD hozzáférési tooEvernote rendelkező szabályozhatja.</span><span class="sxs-lookup"><span data-stu-id="87e0a-106">You can control in Azure AD who has access tooEvernote.</span></span>
- <span data-ttu-id="87e0a-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooEvernote (egyszeri bejelentkezés) a saját Azure AD-fiókok.</span><span class="sxs-lookup"><span data-stu-id="87e0a-107">You can enable your users tooautomatically get signed-on tooEvernote (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="87e0a-108">A fiók egyetlen központi helyen - hello Azure-portálon kezelheti.</span><span class="sxs-lookup"><span data-stu-id="87e0a-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="87e0a-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="87e0a-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="87e0a-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="87e0a-110">Prerequisites</span></span>

<span data-ttu-id="87e0a-111">az Azure AD integrálása Evernote tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="87e0a-111">tooconfigure Azure AD integration with Evernote, you need hello following items:</span></span>

- <span data-ttu-id="87e0a-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="87e0a-112">An Azure AD subscription</span></span>
- <span data-ttu-id="87e0a-113">Egy Evernote egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="87e0a-113">An Evernote single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="87e0a-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="87e0a-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="87e0a-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="87e0a-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="87e0a-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="87e0a-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="87e0a-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="87e0a-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="87e0a-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="87e0a-118">Scenario description</span></span>
<span data-ttu-id="87e0a-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="87e0a-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="87e0a-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="87e0a-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="87e0a-121">Hello gyűjteményből Evernote hozzáadása</span><span class="sxs-lookup"><span data-stu-id="87e0a-121">Adding Evernote from hello gallery</span></span>
2. <span data-ttu-id="87e0a-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="87e0a-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-evernote-from-hello-gallery"></a><span data-ttu-id="87e0a-123">Hello gyűjteményből Evernote hozzáadása</span><span class="sxs-lookup"><span data-stu-id="87e0a-123">Adding Evernote from hello gallery</span></span>
<span data-ttu-id="87e0a-124">tooconfigure hello integrációja Evernote az Azure AD-be, meg kell tooadd Evernote hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="87e0a-124">tooconfigure hello integration of Evernote into Azure AD, you need tooadd Evernote from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="87e0a-125">**tooadd Evernote hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="87e0a-125">**tooadd Evernote from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="87e0a-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="87e0a-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![hello Azure Active Directory gomb][1]

2. <span data-ttu-id="87e0a-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="87e0a-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="87e0a-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="87e0a-129">Then go too**All applications**.</span></span>

    ![hello vállalati alkalmazások panel][2]
    
3. <span data-ttu-id="87e0a-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="87e0a-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![hello új alkalmazás gomb][3]

4. <span data-ttu-id="87e0a-133">Hello keresési mezőbe, írja be a **Evernote**, jelölje be **Evernote** eredmény panelen kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="87e0a-133">In hello search box, type **Evernote**, select **Evernote** from result panel then click **Add** button tooadd hello application.</span></span>

    ![Hello eredménylistában Evernote](./media/active-directory-saas-evernote-tutorial/tutorial_evernote_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="87e0a-135">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="87e0a-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="87e0a-136">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Evernote.</span><span class="sxs-lookup"><span data-stu-id="87e0a-136">In this section, you configure and test Azure AD single sign-on with Evernote based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="87e0a-137">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Evernote tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="87e0a-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Evernote is tooa user in Azure AD.</span></span> <span data-ttu-id="87e0a-138">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Evernote közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="87e0a-138">In other words, a link relationship between an Azure AD user and hello related user in Evernote needs toobe established.</span></span>

<span data-ttu-id="87e0a-139">Evernote, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="87e0a-139">In Evernote, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="87e0a-140">tooconfigure és az Azure AD az egyszeri bejelentkezés Evernote-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="87e0a-140">tooconfigure and test Azure AD single sign-on with Evernote, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="87e0a-141">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="87e0a-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="87e0a-142">**[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="87e0a-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="87e0a-143">**[Hozzon létre egy Evernote tesztfelhasználó](#create-an-evernote-test-user)**  -toohave egy megfelelője a Britta Simon a Evernote, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="87e0a-143">**[Create an Evernote test user](#create-an-evernote-test-user)** - toohave a counterpart of Britta Simon in Evernote that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="87e0a-144">**[Rendelje hozzá az Azure AD hello tesztfelhasználó](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="87e0a-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="87e0a-145">**[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="87e0a-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="87e0a-146">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="87e0a-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="87e0a-147">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Evernote alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="87e0a-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Evernote application.</span></span>

<span data-ttu-id="87e0a-148">**az Azure AD tooconfigure egyszeri bejelentkezést a Evernote, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="87e0a-148">**tooconfigure Azure AD single sign-on with Evernote, perform hello following steps:**</span></span>

1. <span data-ttu-id="87e0a-149">Az Azure portál, a hello hello **Evernote** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="87e0a-149">In hello Azure portal, on hello **Evernote** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés kapcsolat konfigurálása][4]

2. <span data-ttu-id="87e0a-151">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="87e0a-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-evernote-tutorial/tutorial_evernote_samlbase.png)

3. <span data-ttu-id="87e0a-153">A hello **Evernote tartomány és az URL-címek** csoportjában hajtsa végre a következő lépéseket, ha tooconfigure hello alkalmazás IDP kezdeményezett mód hello:</span><span class="sxs-lookup"><span data-stu-id="87e0a-153">On hello **Evernote Domain and URLs** section, perform hello following steps if you wish tooconfigure hello application in IDP initiated mode:</span></span>

    ![Az egyszeri bejelentkezés információk Evernote tartomány és az URL-címek](./media/active-directory-saas-evernote-tutorial/tutorial_evernote_url.png)

    <span data-ttu-id="87e0a-155">A hello **azonosító** szövegmezőhöz típus hello URL-címe:`https://www.evernote.com/saml2`</span><span class="sxs-lookup"><span data-stu-id="87e0a-155">In hello **Identifier** textbox, type hello URL: `https://www.evernote.com/saml2`</span></span>

4. <span data-ttu-id="87e0a-156">Ellenőrizze **megjelenítése speciális URL-beállításainak** , és végezze el a következő lépés, ha tooconfigure hello alkalmazás hello **SP** kezdeményezett mód:</span><span class="sxs-lookup"><span data-stu-id="87e0a-156">Check **Show advanced URL settings** and perform hello following step if you wish tooconfigure hello application in **SP** initiated mode:</span></span>

    ![Az egyszeri bejelentkezés információk Evernote tartomány és az URL-címek](./media/active-directory-saas-evernote-tutorial/tutorial_evernote_url1.png)

    <span data-ttu-id="87e0a-158">A hello **bejelentkezési URL-cím** szövegmezőhöz típus hello URL-címe:`https://www.evernote.com/Login.action`</span><span class="sxs-lookup"><span data-stu-id="87e0a-158">In hello **Sign on URL** textbox, type hello URL: `https://www.evernote.com/Login.action`</span></span>   

5. <span data-ttu-id="87e0a-159">A hello **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="87e0a-159">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![hello tanúsítvány letöltési hivatkozását](./media/active-directory-saas-evernote-tutorial/tutorial_evernote_certificate.png) 

6. <span data-ttu-id="87e0a-161">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="87e0a-161">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-evernote-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="87e0a-163">A hello **Evernote konfigurációs** kattintson **konfigurálása Evernote** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="87e0a-163">On hello **Evernote Configuration** section, click **Configure Evernote** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="87e0a-164">Másolás hello **SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="87e0a-164">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Evernote konfiguráció](./media/active-directory-saas-evernote-tutorial/tutorial_evernote_configure.png) 

8. <span data-ttu-id="87e0a-166">Egy másik webes böngészőablakban jelentkezzen be a Evernote vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="87e0a-166">In a different web browser window, log into your Evernote company site as an administrator.</span></span>

9. <span data-ttu-id="87e0a-167">Nyissa meg túl**"Felügyeleti konzol"**</span><span class="sxs-lookup"><span data-stu-id="87e0a-167">Go too**'Admin Console'**</span></span>

    ![Rendszergazda-konzol](./media/active-directory-saas-evernote-tutorial/tutorial_evernote_adminconsole.png)

10. <span data-ttu-id="87e0a-169">A hello **"Felügyeleti konzol"**, nyissa meg túl**"Security"** válassza ki **"egyszeri bejelentkezéshez"**</span><span class="sxs-lookup"><span data-stu-id="87e0a-169">From hello **'Admin Console'**, go too**‘Security’** and select **‘Single Sign-On’**</span></span>

    ![SSO-beállítás](./media/active-directory-saas-evernote-tutorial/tutorial_evernote_sso.png)

11. <span data-ttu-id="87e0a-171">A következő értékek hello konfigurálása:</span><span class="sxs-lookup"><span data-stu-id="87e0a-171">Configure hello following values:</span></span>

    ![Tanúsítvány-beállítás](./media/active-directory-saas-evernote-tutorial/tutorial_evernote_certx.png)
    
    <span data-ttu-id="87e0a-173">a.</span><span class="sxs-lookup"><span data-stu-id="87e0a-173">a.</span></span>  <span data-ttu-id="87e0a-174">**Egyszeri bejelentkezés engedélyezése:** egyszeri bejelentkezés alapértelmezés szerint engedélyezve van (kattintson **tiltsa le az egyszeri bejelentkezés** tooremove hello SSO követelmény)</span><span class="sxs-lookup"><span data-stu-id="87e0a-174">**Enable SSO:** SSO is enabled by default (Click **Disable Single Sign-on** tooremove hello SSO requirement)</span></span>

    <span data-ttu-id="87e0a-175">b.</span><span class="sxs-lookup"><span data-stu-id="87e0a-175">b.</span></span> <span data-ttu-id="87e0a-176">Beillesztés **SAML-alapú egyszeri bejelentkezést szolgáltatás URL-címe** értéket, amely akkor másolta, az Azure-portálon hello hello **SAML HTTP-kérelmek URL** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="87e0a-176">Paste **SAML Single sign-on Service URL** value, which you have copied from hello Azure portal into hello **SAML HTTP Request URL** textbox.</span></span>

    <span data-ttu-id="87e0a-177">c.</span><span class="sxs-lookup"><span data-stu-id="87e0a-177">c.</span></span> <span data-ttu-id="87e0a-178">Nyissa meg a letöltött tanúsítvány hello Azure AD-t a Jegyzettömbben, és másolja hello tartalommal, beleértve a "BEGIN tanúsítvány" és "END CERTIFICATE", és illessze be hello **X.509 tanúsítvány** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="87e0a-178">Open hello downloaded certificate from Azure AD in a notepad and copy hello content including "BEGIN CERTIFICATE" and "END CERTIFICATE" and paste it into hello **X.509 Certificate** textbox.</span></span> 

    <span data-ttu-id="87e0a-179">d.Click **módosítások mentése**</span><span class="sxs-lookup"><span data-stu-id="87e0a-179">d.Click **Save Changes**</span></span>

> [!TIP]
> <span data-ttu-id="87e0a-180">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="87e0a-180">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="87e0a-181">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="87e0a-181">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="87e0a-182">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="87e0a-182">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="87e0a-183">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="87e0a-183">Create an Azure AD test user</span></span>

<span data-ttu-id="87e0a-184">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="87e0a-184">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Hozzon létre egy Azure AD-teszt felhasználó][100]

<span data-ttu-id="87e0a-186">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="87e0a-186">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="87e0a-187">A hello Azure-portálon, hello bal oldali ablaktáblában kattintson a hello **Azure Active Directory** gombra.</span><span class="sxs-lookup"><span data-stu-id="87e0a-187">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![hello Azure Active Directory gomb](./media/active-directory-saas-evernote-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="87e0a-189">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok**, és kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="87e0a-189">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![hello "Felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-evernote-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="87e0a-191">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello hello tetején **minden felhasználó** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="87e0a-191">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![hello Hozzáadás gomb](./media/active-directory-saas-evernote-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="87e0a-193">A hello **felhasználói** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="87e0a-193">In hello **User** dialog box, perform hello following steps:</span></span>

    ![hello felhasználó párbeszédpanel](./media/active-directory-saas-evernote-tutorial/create_aaduser_04.png)

    <span data-ttu-id="87e0a-195">a.</span><span class="sxs-lookup"><span data-stu-id="87e0a-195">a.</span></span> <span data-ttu-id="87e0a-196">A hello **neve** mezőbe írja be **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="87e0a-196">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="87e0a-197">b.</span><span class="sxs-lookup"><span data-stu-id="87e0a-197">b.</span></span> <span data-ttu-id="87e0a-198">A hello **felhasználónév** mezőben, a felhasználó Britta Simon típus hello e-mail címét.</span><span class="sxs-lookup"><span data-stu-id="87e0a-198">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="87e0a-199">c.</span><span class="sxs-lookup"><span data-stu-id="87e0a-199">c.</span></span> <span data-ttu-id="87e0a-200">Jelölje be hello **megjelenítése jelszó** jelölje be a jelölőnégyzetet, és jegyezze fel a hello hello érték **jelszó** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="87e0a-200">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="87e0a-201">d.</span><span class="sxs-lookup"><span data-stu-id="87e0a-201">d.</span></span> <span data-ttu-id="87e0a-202">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="87e0a-202">Click **Create**.</span></span>
 
### <a name="create-an-evernote-test-user"></a><span data-ttu-id="87e0a-203">Hozzon létre egy Evernote tesztfelhasználó számára</span><span class="sxs-lookup"><span data-stu-id="87e0a-203">Create an Evernote test user</span></span>

<span data-ttu-id="87e0a-204">A sorrend tooenable az Azure AD felhasználók toolog Evernote be azok ki kell építenie Evernote be.</span><span class="sxs-lookup"><span data-stu-id="87e0a-204">In order tooenable Azure AD users toolog into Evernote, they must be provisioned into Evernote.</span></span>  
<span data-ttu-id="87e0a-205">Evernote hello esetben egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="87e0a-205">In hello case of Evernote, provisioning is a manual task.</span></span>

<span data-ttu-id="87e0a-206">**tooprovision felhasználói fiókok, hajtsa végre hello a következő lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="87e0a-206">**tooprovision a user accounts, perform hello following steps:**</span></span>

1. <span data-ttu-id="87e0a-207">Jelentkezzen be tooyour Evernote vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="87e0a-207">Log in tooyour Evernote company site as an administrator.</span></span>

2. <span data-ttu-id="87e0a-208">Kattintson a hello **"Felügyeleti konzol"**.</span><span class="sxs-lookup"><span data-stu-id="87e0a-208">Click hello **'Admin Console'**.</span></span>

    ![Rendszergazda-konzol](./media/active-directory-saas-evernote-tutorial/tutorial_evernote_adminconsole.png)

3. <span data-ttu-id="87e0a-210">A hello **"Felügyeleti konzol"**, nyissa meg túl**"Felhasználók hozzáadása"**.</span><span class="sxs-lookup"><span data-stu-id="87e0a-210">From hello **'Admin Console'**, go too**‘Add users’**.</span></span>

    ![Tesztfelhasználó-nevet](./media/active-directory-saas-evernote-tutorial/create_aaduser_0001.png)

4. <span data-ttu-id="87e0a-212">**Adja hozzá a csoport tagjai** a hello **E-mail** szövegmező, írja be a felhasználói fiók hello e-mail címet, majd kattintson **hívhat meg.**</span><span class="sxs-lookup"><span data-stu-id="87e0a-212">**Add team members** in hello **Email** textbox, type hello email address of user account and click **Invite.**</span></span>

    ![Tesztfelhasználó-nevet](./media/active-directory-saas-evernote-tutorial/create_aaduser_0002.png)
    
5. <span data-ttu-id="87e0a-214">Meghívót küldött, miután hello Azure Active Directory fióktulajdonos e-mail tooaccept hello meghívót fog kapni.</span><span class="sxs-lookup"><span data-stu-id="87e0a-214">After invitation is sent, hello Azure Active Directory account holder will receive an email tooaccept hello invitation.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="87e0a-215">Rendelje hozzá az Azure AD hello tesztfelhasználó számára</span><span class="sxs-lookup"><span data-stu-id="87e0a-215">Assign hello Azure AD test user</span></span>

<span data-ttu-id="87e0a-216">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooEvernote megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="87e0a-216">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooEvernote.</span></span>

![Hello felhasználói szerepkör hozzárendelése][200] 

<span data-ttu-id="87e0a-218">**tooassign Britta Simon tooEvernote, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="87e0a-218">**tooassign Britta Simon tooEvernote, perform hello following steps:**</span></span>

1. <span data-ttu-id="87e0a-219">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="87e0a-219">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="87e0a-221">Hello alkalmazások listában válassza ki a **Evernote**.</span><span class="sxs-lookup"><span data-stu-id="87e0a-221">In hello applications list, select **Evernote**.</span></span>

    ![hello Evernote hivatkozásra hello alkalmazások listája](./media/active-directory-saas-evernote-tutorial/tutorial_evernote_app.png)  

3. <span data-ttu-id="87e0a-223">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="87e0a-223">In hello menu on hello left, click **Users and groups**.</span></span>

    ![hello "Felhasználók és csoportok" hivatkozásra.][202]

4. <span data-ttu-id="87e0a-225">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="87e0a-225">Click **Add** button.</span></span> <span data-ttu-id="87e0a-226">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="87e0a-226">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![hello hozzárendelés hozzáadása panelen][203]

5. <span data-ttu-id="87e0a-228">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="87e0a-228">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="87e0a-229">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="87e0a-229">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="87e0a-230">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="87e0a-230">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="87e0a-231">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="87e0a-231">Test single sign-on</span></span>

<span data-ttu-id="87e0a-232">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="87e0a-232">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="87e0a-233">Ha a hozzáférési Panel hello hello Evernote csempe gombra kattint, bejelentkezett tooyour Evernote alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="87e0a-233">When you click hello Evernote tile in hello Access Panel, you should get signed-on tooyour Evernote application.</span></span> <span data-ttu-id="87e0a-234">Akkor lesz naplózása egy szervezeti fiók, de a szükséges toolog be a személyes fiókjával.</span><span class="sxs-lookup"><span data-stu-id="87e0a-234">You'll be logging in as an Organization account but then need toolog in with your personal account.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="87e0a-235">További források</span><span class="sxs-lookup"><span data-stu-id="87e0a-235">Additional resources</span></span>

* [<span data-ttu-id="87e0a-236">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="87e0a-236">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="87e0a-237">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="87e0a-237">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-evernote-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-evernote-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-evernote-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-evernote-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-evernote-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-evernote-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-evernote-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-evernote-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-evernote-tutorial/tutorial_general_203.png

