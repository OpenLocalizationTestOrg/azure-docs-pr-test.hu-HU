---
title: "Oktatóanyag: Azure Active Directoryval integrált TigerText biztonságos Messenger |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és TigerText biztonságos Messenger között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 03f1e128-5bcb-4e49-b6a3-fe22eedc6d5e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: jeedes
ms.openlocfilehash: a3d7bb9598658c75c567c15751740d885fe4fc27
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-tigertext-secure-messenger"></a><span data-ttu-id="45589-103">Oktatóanyag: Azure Active Directoryval integrált TigerText biztonságos Messenger</span><span class="sxs-lookup"><span data-stu-id="45589-103">Tutorial: Azure Active Directory integration with TigerText Secure Messenger</span></span>

<span data-ttu-id="45589-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate TigerText biztonságos Messenger az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="45589-104">In this tutorial, you learn how toointegrate TigerText Secure Messenger with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="45589-105">TigerText biztonságos Messenger integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="45589-105">Integrating TigerText Secure Messenger with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="45589-106">Szabályozhatja az Azure AD hozzáférési tooTigerText rendelkező biztonságos Messenger</span><span class="sxs-lookup"><span data-stu-id="45589-106">You can control in Azure AD who has access tooTigerText Secure Messenger</span></span>
- <span data-ttu-id="45589-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooTigerText biztonságos Messenger (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="45589-107">You can enable your users tooautomatically get signed-on tooTigerText Secure Messenger (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="45589-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="45589-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="45589-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="45589-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="45589-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="45589-110">Prerequisites</span></span>

<span data-ttu-id="45589-111">tooconfigure az Azure AD integrálása TigerText biztonságos Messenger, a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="45589-111">tooconfigure Azure AD integration with TigerText Secure Messenger, you need hello following items:</span></span>

- <span data-ttu-id="45589-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="45589-112">An Azure AD subscription</span></span>
- <span data-ttu-id="45589-113">Egy TigerText biztonságos Messenger egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="45589-113">A TigerText Secure Messenger single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="45589-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="45589-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="45589-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="45589-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="45589-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="45589-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="45589-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="45589-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="45589-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="45589-118">Scenario description</span></span>
<span data-ttu-id="45589-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="45589-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="45589-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="45589-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="45589-121">Adja hozzá a TigerText biztonságos Messenger hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="45589-121">Add TigerText Secure Messenger from hello gallery</span></span>
2. <span data-ttu-id="45589-122">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="45589-122">Configure and test Azure AD single sign-on</span></span>

## <a name="add-tigertext-secure-messenger-from-hello-gallery"></a><span data-ttu-id="45589-123">Adja hozzá a TigerText biztonságos Messenger hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="45589-123">Add TigerText Secure Messenger from hello gallery</span></span>
<span data-ttu-id="45589-124">tooconfigure hello integrációs TigerText biztonságos Messenger, az Azure AD-be, meg kell tooadd TigerText biztonságos Messenger hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="45589-124">tooconfigure hello integration of TigerText Secure Messenger into Azure AD, you need tooadd TigerText Secure Messenger from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="45589-125">**tooadd TigerText biztonságos Messenger hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="45589-125">**tooadd TigerText Secure Messenger from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="45589-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="45589-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="45589-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="45589-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="45589-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="45589-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="45589-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="45589-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="45589-133">Hello keresési mezőbe, írja be a **TigerText biztonságos Messenger**, jelölje be **TigerText biztonságos Messenger** eredmény panelen kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="45589-133">In hello search box, type **TigerText Secure Messenger**, select **TigerText Secure Messenger** from result panel then click **Add** button tooadd hello application.</span></span>

    ![Adja hozzá a gyűjteményből](./media/active-directory-saas-tigertext-tutorial/tutorial_tigertext_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="45589-135">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="45589-135">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="45589-136">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD az egyszeri bejelentkezés TigerText biztonságos Messenger "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="45589-136">In this section, you configure and test Azure AD single sign-on with TigerText Secure Messenger based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="45589-137">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói TigerText biztonságos Messenger tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="45589-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in TigerText Secure Messenger is tooa user in Azure AD.</span></span> <span data-ttu-id="45589-138">Ez azt jelenti hello kapcsolódó felhasználói TigerText biztonságos Messenger és az Azure AD-felhasználó közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="45589-138">In other words, a link relationship between an Azure AD user and hello related user in TigerText Secure Messenger needs toobe established.</span></span>

<span data-ttu-id="45589-139">A biztonságos TigerText Messenger rendelje hello hello értékét **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="45589-139">In TigerText Secure Messenger, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="45589-140">tooconfigure és az Azure AD az egyszeri bejelentkezés TigerText biztonságos Messenger-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="45589-140">tooconfigure and test Azure AD single sign-on with TigerText Secure Messenger, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="45589-141">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="45589-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="45589-142">**[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="45589-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="45589-143">**[TigerText biztonságos Messenger tesztfelhasználó létrehozása](#create-a-tigertext-secure-messenger-test-user)**  -toohave egy megfelelője a Britta Simon TigerText biztonságos Messenger, a felhasználó csatolt toohello az Azure AD-ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="45589-143">**[Create a TigerText Secure Messenger test user](#create-a-tigertext-secure-messenger-test-user)** - toohave a counterpart of Britta Simon in TigerText Secure Messenger that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="45589-144">**[Rendelje hozzá az Azure AD hello tesztfelhasználó](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="45589-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="45589-145">**[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="45589-145">**[Test Single Sign-On](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="45589-146">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="45589-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="45589-147">Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése, és az TigerText biztonságos Messenger alkalmazás egyszeri bejelentkezés beállítása.</span><span class="sxs-lookup"><span data-stu-id="45589-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your TigerText Secure Messenger application.</span></span>

<span data-ttu-id="45589-148">**az Azure AD az egyszeri bejelentkezés tooconfigure TigerText biztonságos Messenger, hajtsa végre a hello a következő lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="45589-148">**tooconfigure Azure AD single sign-on with TigerText Secure Messenger, perform hello following steps:**</span></span>

1. <span data-ttu-id="45589-149">Az Azure portál, a hello hello **TigerText biztonságos Messenger** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="45589-149">In hello Azure portal, on hello **TigerText Secure Messenger** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="45589-151">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="45589-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![SAML-alapú bejelentkezés](./media/active-directory-saas-tigertext-tutorial/tutorial_tigertext_samlbase.png)

3. <span data-ttu-id="45589-153">A hello **TigerText biztonságos Messenger tartománya és URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="45589-153">On hello **TigerText Secure Messenger Domain and URLs** section, perform hello following steps:</span></span>

    ![TigerText biztonságos Messenger tartománya és URL-címek szakasz](./media/active-directory-saas-tigertext-tutorial/tutorial_tigertext_url.png)

    <span data-ttu-id="45589-155">a.</span><span class="sxs-lookup"><span data-stu-id="45589-155">a.</span></span> <span data-ttu-id="45589-156">A hello **bejelentkezési URL-cím** szövegmezőhöz típus az URL-címet:`https://home.tigertext.com`</span><span class="sxs-lookup"><span data-stu-id="45589-156">In hello **Sign-on URL** textbox, type URL as: `https://home.tigertext.com`</span></span>

    <span data-ttu-id="45589-157">b.</span><span class="sxs-lookup"><span data-stu-id="45589-157">b.</span></span> <span data-ttu-id="45589-158">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://saml-lb.tigertext.me/v1/organization/<instance Id>`</span><span class="sxs-lookup"><span data-stu-id="45589-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://saml-lb.tigertext.me/v1/organization/<instance Id>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="45589-159">Ez az érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="45589-159">This value is not real.</span></span> <span data-ttu-id="45589-160">Frissítse ezt az értéket hello tényleges azonosítója.</span><span class="sxs-lookup"><span data-stu-id="45589-160">Update this value with hello actual Identifier.</span></span> <span data-ttu-id="45589-161">Ügyfél [TigerText biztonságos Messenger ügyfél-támogatási csoport](mailTo:prosupport@tigertext.com) tooget ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="45589-161">Contact [TigerText Secure Messenger Client support team](mailTo:prosupport@tigertext.com) tooget this value.</span></span> 
 
4. <span data-ttu-id="45589-162">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="45589-162">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![SAML-aláíró tanúsítványa szakasz](./media/active-directory-saas-tigertext-tutorial/tutorial_tigertext_certificate.png) 

5. <span data-ttu-id="45589-164">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="45589-164">Click **Save** button.</span></span>

    ![Mentés gomb](./media/active-directory-saas-tigertext-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="45589-166">tooget egyszeri bejelentkezést az alkalmazás, ügyfél konfigurált [TigerText biztonságos Messenger támogatási csoport](mailTo:prosupport@tigertext.com) , és adja meg azokat a hello **letöltött metaadatok**.</span><span class="sxs-lookup"><span data-stu-id="45589-166">tooget single sign-on configured for your application, contact [TigerText Secure Messenger support team](mailTo:prosupport@tigertext.com) and provide them hello **Downloaded metadata**.</span></span>

> [!TIP]
> <span data-ttu-id="45589-167">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="45589-167">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="45589-168">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="45589-168">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="45589-169">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="45589-169">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="45589-170">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="45589-170">Create an Azure AD test user</span></span>
<span data-ttu-id="45589-171">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="45589-171">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="45589-173">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="45589-173">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="45589-174">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="45589-174">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-tigertext-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="45589-176">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="45589-176">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Felhasználók és csoportok -> minden felhasználó](./media/active-directory-saas-tigertext-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="45589-178">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="45589-178">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Hozzáadás gomb](./media/active-directory-saas-tigertext-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="45589-180">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="45589-180">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Felhasználó párbeszédpanel](./media/active-directory-saas-tigertext-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="45589-182">a.</span><span class="sxs-lookup"><span data-stu-id="45589-182">a.</span></span> <span data-ttu-id="45589-183">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="45589-183">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="45589-184">b.</span><span class="sxs-lookup"><span data-stu-id="45589-184">b.</span></span> <span data-ttu-id="45589-185">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="45589-185">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="45589-186">c.</span><span class="sxs-lookup"><span data-stu-id="45589-186">c.</span></span> <span data-ttu-id="45589-187">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="45589-187">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="45589-188">d.</span><span class="sxs-lookup"><span data-stu-id="45589-188">d.</span></span> <span data-ttu-id="45589-189">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="45589-189">Click **Create**.</span></span>
 
### <a name="create-a-tigertext-secure-messenger-test-user"></a><span data-ttu-id="45589-190">TigerText biztonságos Messenger tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="45589-190">Create a TigerText Secure Messenger test user</span></span>

<span data-ttu-id="45589-191">Ebben a szakaszban egy TigerText Britta Simon nevű felhasználót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="45589-191">In this section, you create a user called Britta Simon in TigerText.</span></span> <span data-ttu-id="45589-192">Lépjen kapcsolatba túl[TigerText biztonságos Messenger ügyfél-támogatási csoport](mailTo:prosupport@tigertext.com) tooadd hello felhasználók hello TigerText platform.</span><span class="sxs-lookup"><span data-stu-id="45589-192">Please reach out too[TigerText Secure Messenger Client support team](mailTo:prosupport@tigertext.com) tooadd hello users in hello TigerText platform.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="45589-193">Rendelje hozzá az Azure AD hello tesztfelhasználó számára</span><span class="sxs-lookup"><span data-stu-id="45589-193">Assign hello Azure AD test user</span></span>

<span data-ttu-id="45589-194">Ebben a szakaszban hozzáférés tooTigerText megadásával engedélyeznie Britta Simon toouse Azure egyszeri bejelentkezés Messenger biztonságos.</span><span class="sxs-lookup"><span data-stu-id="45589-194">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooTigerText Secure Messenger.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="45589-196">**tooassign Britta Simon tooTigerText biztonságos Messenger, hajtsa végre az alábbi lépésekkel hello:**</span><span class="sxs-lookup"><span data-stu-id="45589-196">**tooassign Britta Simon tooTigerText Secure Messenger, perform hello following steps:**</span></span>

1. <span data-ttu-id="45589-197">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="45589-197">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="45589-199">Hello alkalmazások listában válassza ki a **TigerText biztonságos Messenger**.</span><span class="sxs-lookup"><span data-stu-id="45589-199">In hello applications list, select **TigerText Secure Messenger**.</span></span>

    ![TigerText biztonságos Messenger alkalmazáslistában](./media/active-directory-saas-tigertext-tutorial/tutorial_tigertext_app.png) 

3. <span data-ttu-id="45589-201">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="45589-201">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="45589-203">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="45589-203">Click **Add** button.</span></span> <span data-ttu-id="45589-204">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="45589-204">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="45589-206">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="45589-206">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="45589-207">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="45589-207">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="45589-208">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="45589-208">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="45589-209">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="45589-209">Test single sign-on</span></span>

<span data-ttu-id="45589-210">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="45589-210">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="45589-211">Ha a hozzáférési Panel hello hello TigerText csempe gombra kattint, automatikusan bejelentkezett tooyour TigerText alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="45589-211">When you click hello TigerText tile in hello Access Panel, you should get automatically signed-on tooyour TigerText application.</span></span> <span data-ttu-id="45589-212">További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="45589-212">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="45589-213">További források</span><span class="sxs-lookup"><span data-stu-id="45589-213">Additional resources</span></span>

* [<span data-ttu-id="45589-214">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="45589-214">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="45589-215">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="45589-215">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-tigertext-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-tigertext-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-tigertext-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-tigertext-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-tigertext-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-tigertext-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-tigertext-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-tigertext-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-tigertext-tutorial/tutorial_general_203.png

