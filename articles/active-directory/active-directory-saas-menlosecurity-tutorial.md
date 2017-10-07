---
title: "Oktatóanyag: Azure Active Directoryval integrált Menlo biztonsági |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Menlo biztonsági között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 9e63fe6b-0ad0-405d-9e41-6a1a40a41df8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2017
ms.author: jeedes
ms.openlocfilehash: 193d12eedf31f4f08e1d141936d6e918c36a2109
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-menlo-security"></a><span data-ttu-id="d9493-103">Oktatóanyag: Azure Active Directoryval integrált Menlo biztonsági</span><span class="sxs-lookup"><span data-stu-id="d9493-103">Tutorial: Azure Active Directory integration with Menlo Security</span></span>

<span data-ttu-id="d9493-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Menlo biztonsági az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="d9493-104">In this tutorial, you learn how toointegrate Menlo Security with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="d9493-105">Menlo biztonsági integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="d9493-105">Integrating Menlo Security with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="d9493-106">Megadhatja a hozzáférés tooMenlo biztonsági rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="d9493-106">You can control in Azure AD who has access tooMenlo Security</span></span>
- <span data-ttu-id="d9493-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooMenlo biztonsági (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="d9493-107">You can enable your users tooautomatically get signed-on tooMenlo Security (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="d9493-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="d9493-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="d9493-109">Ha azt szeretné, tooknow SaaS alkalmazásintegráció az Azure AD-vel kapcsolatos további részletekért lásd:.</span><span class="sxs-lookup"><span data-stu-id="d9493-109">If you want tooknow more details about SaaS app integration with Azure AD, see.</span></span> <span data-ttu-id="d9493-110">[Alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="d9493-110">[What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d9493-111">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="d9493-111">Prerequisites</span></span>

<span data-ttu-id="d9493-112">az Azure AD integrálása Menlo biztonsági tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="d9493-112">tooconfigure Azure AD integration with Menlo Security, you need hello following items:</span></span>

- <span data-ttu-id="d9493-113">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="d9493-113">An Azure AD subscription</span></span>
- <span data-ttu-id="d9493-114">Egy Menlo biztonsági egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="d9493-114">A Menlo Security single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="d9493-115">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="d9493-115">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="d9493-116">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="d9493-116">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="d9493-117">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="d9493-117">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="d9493-118">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="d9493-118">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="d9493-119">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="d9493-119">Scenario description</span></span>
<span data-ttu-id="d9493-120">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="d9493-120">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="d9493-121">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="d9493-121">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="d9493-122">Hello gyűjteményből Menlo adatvédelme</span><span class="sxs-lookup"><span data-stu-id="d9493-122">Adding Menlo Security from hello gallery</span></span>
2. <span data-ttu-id="d9493-123">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="d9493-123">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-menlo-security-from-hello-gallery"></a><span data-ttu-id="d9493-124">Hello gyűjteményből Menlo adatvédelme</span><span class="sxs-lookup"><span data-stu-id="d9493-124">Adding Menlo Security from hello gallery</span></span>
<span data-ttu-id="d9493-125">tooconfigure hello integrációs Menlo biztonsági az Azure AD-be, meg kell tooadd Menlo biztonsági hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="d9493-125">tooconfigure hello integration of Menlo Security into Azure AD, you need tooadd Menlo Security from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="d9493-126">**tooadd Menlo biztonsági hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="d9493-126">**tooadd Menlo Security from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="d9493-127">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="d9493-127">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="d9493-129">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="d9493-129">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="d9493-130">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="d9493-130">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="d9493-132">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="d9493-132">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="d9493-134">Hello keresési mezőbe, írja be a **Menlo biztonsági**.</span><span class="sxs-lookup"><span data-stu-id="d9493-134">In hello search box, type **Menlo Security**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-menlosecurity-tutorial/tutorial_menlosecurity_search.png)

5. <span data-ttu-id="d9493-136">A hello eredmények panelen válassza ki a **Menlo biztonsági**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="d9493-136">In hello results panel, select **Menlo Security**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-menlosecurity-tutorial/tutorial_menlosecurity_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="d9493-138">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="d9493-138">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="d9493-139">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján Menlo biztonság</span><span class="sxs-lookup"><span data-stu-id="d9493-139">In this section, you configure and test Azure AD single sign-on with Menlo Security based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="d9493-140">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói Menlo biztonsági tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="d9493-140">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Menlo Security is tooa user in Azure AD.</span></span> <span data-ttu-id="d9493-141">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználói hello Menlo biztonsági közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="d9493-141">In other words, a link relationship between an Azure AD user and hello related user in Menlo Security needs toobe established.</span></span>

<span data-ttu-id="d9493-142">Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** Menlo biztonsági.</span><span class="sxs-lookup"><span data-stu-id="d9493-142">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Menlo Security.</span></span>

<span data-ttu-id="d9493-143">tooconfigure és az Azure AD az egyszeri bejelentkezés Menlo biztonsági-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="d9493-143">tooconfigure and test Azure AD single sign-on with Menlo Security, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="d9493-144">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="d9493-144">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="d9493-145">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d9493-145">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="d9493-146">**[Menlo biztonsági tesztfelhasználó létrehozása](#creating-a-menlo-security-test-user)**  -toohave egy megfelelője a Britta Simon Menlo biztonsági, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="d9493-146">**[Creating a Menlo Security test user](#creating-a-menlo-security-test-user)** - toohave a counterpart of Britta Simon in Menlo Security that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="d9493-147">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="d9493-147">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="d9493-148">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="d9493-148">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="d9493-149">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="d9493-149">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="d9493-150">Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése, és az Menlo biztonsági alkalmazás egyszeri bejelentkezés beállítása.</span><span class="sxs-lookup"><span data-stu-id="d9493-150">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Menlo Security application.</span></span>

<span data-ttu-id="d9493-151">**az Azure AD tooconfigure egyszeri bejelentkezés Menlo biztonsági, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="d9493-151">**tooconfigure Azure AD single sign-on with Menlo Security, perform hello following steps:**</span></span>

1. <span data-ttu-id="d9493-152">Az Azure portál, a hello hello **Menlo biztonsági** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="d9493-152">In hello Azure portal, on hello **Menlo Security** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="d9493-154">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="d9493-154">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-menlosecurity-tutorial/tutorial_menlosecurity_samlbase.png)

3. <span data-ttu-id="d9493-156">A hello **Menlo biztonsági tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="d9493-156">On hello **Menlo Security Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-menlosecurity-tutorial/tutorial_menlosecurity_url.png)

    <span data-ttu-id="d9493-158">a.</span><span class="sxs-lookup"><span data-stu-id="d9493-158">a.</span></span> <span data-ttu-id="d9493-159">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<subdomain>.menlosecurity.com/account/login`</span><span class="sxs-lookup"><span data-stu-id="d9493-159">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<subdomain>.menlosecurity.com/account/login`</span></span>

    <span data-ttu-id="d9493-160">b.</span><span class="sxs-lookup"><span data-stu-id="d9493-160">b.</span></span> <span data-ttu-id="d9493-161">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<subdomain>.menlosecurity.com/safeview-auth-server/saml/metadata`</span><span class="sxs-lookup"><span data-stu-id="d9493-161">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<subdomain>.menlosecurity.com/safeview-auth-server/saml/metadata`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="d9493-162">Ezek az értékek nincsenek hello valós.</span><span class="sxs-lookup"><span data-stu-id="d9493-162">These values are not hello real.</span></span> <span data-ttu-id="d9493-163">Frissítse a bejelentkezési URL-cím és azonosító a hello tényleges értékek.</span><span class="sxs-lookup"><span data-stu-id="d9493-163">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="d9493-164">Ügyfél [Menlo biztonsági ügyfél-támogatási csoport](https://www.menlosecurity.com/menlo-contact) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="d9493-164">Contact [Menlo Security Client support team](https://www.menlosecurity.com/menlo-contact) tooget these values.</span></span> 
 
4. <span data-ttu-id="d9493-165">A hello **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="d9493-165">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello Certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-menlosecurity-tutorial/tutorial_menlosecurity_certificate.png) 

5. <span data-ttu-id="d9493-167">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="d9493-167">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-menlosecurity-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="d9493-169">A hello **Menlo biztonsági konfiguráció** kattintson **Menlo biztonságának konfigurálása** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="d9493-169">On hello **Menlo Security Configuration** section, click **Configure Menlo Security** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="d9493-170">Másolás hello **SAML Entitásazonosító**, és **SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="d9493-170">Copy hello **SAML Entity ID**, and **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-menlosecurity-tutorial/tutorial_menlosecurity_configure.png) 

7. <span data-ttu-id="d9493-172">tooconfigure egyszeri bejelentkezést a **Menlo biztonsági** oldalon, a bejelentkezési toohello **Menlo biztonsági** webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="d9493-172">tooconfigure single sign-on on **Menlo Security** side, login toohello **Menlo Security** website as an administrator.</span></span>

8. <span data-ttu-id="d9493-173">A **beállítások** lépjen túl**hitelesítési** , és végezze el a következő műveleteket:</span><span class="sxs-lookup"><span data-stu-id="d9493-173">Under **Settings** go too**Authentication** and perform following actions:</span></span>
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-menlosecurity-tutorial/menlo_user_setup.png)

    <span data-ttu-id="d9493-175">a.</span><span class="sxs-lookup"><span data-stu-id="d9493-175">a.</span></span> <span data-ttu-id="d9493-176">Osztásjelek hello jelölőnégyzet **használatával SAML-alapú hitelesítés engedélyezése felhasználói**.</span><span class="sxs-lookup"><span data-stu-id="d9493-176">Tick hello checkbox **Enable user authentication using SAML**.</span></span>

    <span data-ttu-id="d9493-177">b.</span><span class="sxs-lookup"><span data-stu-id="d9493-177">b.</span></span> <span data-ttu-id="d9493-178">Válassza ki **külső hozzáférés engedélyezése** túl**Igen**.</span><span class="sxs-lookup"><span data-stu-id="d9493-178">Select **Allow External Access** too**Yes**.</span></span>

    <span data-ttu-id="d9493-179">c.</span><span class="sxs-lookup"><span data-stu-id="d9493-179">c.</span></span> <span data-ttu-id="d9493-180">A **SAML-szolgáltató**, jelölje be **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="d9493-180">Under **SAML Provider**, select **Azure Active Directory**.</span></span>

    <span data-ttu-id="d9493-181">d.</span><span class="sxs-lookup"><span data-stu-id="d9493-181">d.</span></span> <span data-ttu-id="d9493-182">**SAML 2.0 végpont** : Beillesztés hello **SAML-alapú egyszeri bejelentkezési URL-címe** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="d9493-182">**SAML 2.0 Endpoint** : Paste hello **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="d9493-183">e.</span><span class="sxs-lookup"><span data-stu-id="d9493-183">e.</span></span> <span data-ttu-id="d9493-184">**Szolgáltatás azonosítója (kibocsátó)** : Beillesztés hello **SAML Entitásazonosító** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="d9493-184">**Service Identifier (Issuer)** : Paste hello **SAML Entity ID** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="d9493-185">f.</span><span class="sxs-lookup"><span data-stu-id="d9493-185">f.</span></span> <span data-ttu-id="d9493-186">**X.509 tanúsítvány** : Nyissa meg hello **tanúsítvány (Base64)** le: hello Azure-portál a Jegyzettömbben, és illessze be ezt a jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="d9493-186">**X.509 Certificate** : Open hello **Certificate (Base64)** downloaded from hello Azure Portal in notepad and paste it in this box.</span></span>

    <span data-ttu-id="d9493-187">g.</span><span class="sxs-lookup"><span data-stu-id="d9493-187">g.</span></span> <span data-ttu-id="d9493-188">Kattintson a **mentése** toosave hello beállításait.</span><span class="sxs-lookup"><span data-stu-id="d9493-188">Click **Save** toosave hello settings.</span></span>

> [!TIP]
> <span data-ttu-id="d9493-189">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="d9493-189">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="d9493-190">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="d9493-190">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="d9493-191">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="d9493-191">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="d9493-192">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="d9493-192">Creating an Azure AD test user</span></span>
<span data-ttu-id="d9493-193">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="d9493-193">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="d9493-195">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="d9493-195">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="d9493-196">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="d9493-196">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-menlosecurity-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="d9493-198">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="d9493-198">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-menlosecurity-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="d9493-200">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d9493-200">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-menlosecurity-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="d9493-202">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="d9493-202">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-menlosecurity-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="d9493-204">a.</span><span class="sxs-lookup"><span data-stu-id="d9493-204">a.</span></span> <span data-ttu-id="d9493-205">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="d9493-205">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="d9493-206">b.</span><span class="sxs-lookup"><span data-stu-id="d9493-206">b.</span></span> <span data-ttu-id="d9493-207">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="d9493-207">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="d9493-208">c.</span><span class="sxs-lookup"><span data-stu-id="d9493-208">c.</span></span> <span data-ttu-id="d9493-209">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="d9493-209">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="d9493-210">d.</span><span class="sxs-lookup"><span data-stu-id="d9493-210">d.</span></span> <span data-ttu-id="d9493-211">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="d9493-211">Click **Create**.</span></span>
 
### <a name="creating-a-menlo-security-test-user"></a><span data-ttu-id="d9493-212">Menlo biztonsági tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="d9493-212">Creating a Menlo Security test user</span></span>
 
<span data-ttu-id="d9493-213">Ebben a szakaszban egy Menlo biztonsági Britta Simon nevű felhasználót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="d9493-213">In this section, you create a user called Britta Simon in Menlo Security.</span></span> <span data-ttu-id="d9493-214">Együttműködve [Menlo biztonsági ügyfél-támogatási csoport](https://www.menlosecurity.com/menlo-contact) tooadd hello felhasználók hello Menlo biztonsági platform.</span><span class="sxs-lookup"><span data-stu-id="d9493-214">Work with [Menlo Security Client support team](https://www.menlosecurity.com/menlo-contact) tooadd hello users in hello Menlo Security platform.</span></span> <span data-ttu-id="d9493-215">Felhasználók kell létrehoznia és aktiválni az egyszeri bejelentkezés használata előtt.</span><span class="sxs-lookup"><span data-stu-id="d9493-215">Users must be created and activated before you use single sign-on.</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="d9493-216">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="d9493-216">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="d9493-217">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooMenlo biztonsági megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="d9493-217">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooMenlo Security.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="d9493-219">**tooassign Britta Simon tooMenlo biztonsági, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="d9493-219">**tooassign Britta Simon tooMenlo Security, perform hello following steps:**</span></span>

1. <span data-ttu-id="d9493-220">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="d9493-220">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="d9493-222">Hello alkalmazások listában válassza ki a **Menlo biztonsági**.</span><span class="sxs-lookup"><span data-stu-id="d9493-222">In hello applications list, select **Menlo Security**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-menlosecurity-tutorial/tutorial_menlosecurity_app.png) 

3. <span data-ttu-id="d9493-224">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="d9493-224">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="d9493-226">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="d9493-226">Click **Add** button.</span></span> <span data-ttu-id="d9493-227">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d9493-227">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="d9493-229">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="d9493-229">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="d9493-230">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d9493-230">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="d9493-231">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d9493-231">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="d9493-232">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="d9493-232">Testing single sign-on</span></span>

<span data-ttu-id="d9493-233">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai tesztelése.</span><span class="sxs-lookup"><span data-stu-id="d9493-233">In this section, you test your Azure AD single sign-on configuration.</span></span>

<span data-ttu-id="d9493-234">Nyisson meg egy böngészőablakot az egy "InPrivate" vagy "Incognito" mód tootrigger egy új hitelesítési.</span><span class="sxs-lookup"><span data-stu-id="d9493-234">Open a browser window in an "InPrivate" or "Incognito" mode tootrigger a new authentication.</span></span>  <span data-ttu-id="d9493-235">Az Internet Explorerben használja a Ctrl + Shift + P.</span><span class="sxs-lookup"><span data-stu-id="d9493-235">In Internet Explorer, use Ctrl+Shift+P.</span></span>  <span data-ttu-id="d9493-236">A Chrome-ban használja a Ctrl + Shift + N.</span><span class="sxs-lookup"><span data-stu-id="d9493-236">In Chrome, use Ctrl+Shift+N.</span></span>  <span data-ttu-id="d9493-237">Hello privát böngészés ablakban keresse meg a védett tooa erőforrás, és hajtsa végre az Azure AD bejelentkezés.</span><span class="sxs-lookup"><span data-stu-id="d9493-237">In hello private browsing window, browse tooa protected resource and perform an Azure AD login.</span></span>  <span data-ttu-id="d9493-238">Sikeres bejelentkezés esetén tett toohello kért hely fogja elkülönítési munkamenetekben.</span><span class="sxs-lookup"><span data-stu-id="d9493-238">Upon successful login, you will be taken toohello requested site in an isolation session.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d9493-239">További források</span><span class="sxs-lookup"><span data-stu-id="d9493-239">Additional resources</span></span>

* [<span data-ttu-id="d9493-240">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="d9493-240">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="d9493-241">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="d9493-241">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-menlosecurity-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-menlosecurity-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-menlosecurity-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-menlosecurity-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-menlosecurity-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-menlosecurity-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-menlosecurity-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-menlosecurity-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-menlosecurity-tutorial/tutorial_general_203.png

