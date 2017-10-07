---
title: "Oktatóanyag: Azure Active Directoryval integrált Wingspan eTMF |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Wingspan eTMF között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: ace320d3-521c-449c-992f-feabe7538de7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: jeedes
ms.openlocfilehash: ed5fda5318a0d3841af8b2db4fb74db550befaea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-wingspan-etmf"></a><span data-ttu-id="a63b1-103">Oktatóanyag: Azure Active Directoryval integrált Wingspan eTMF</span><span class="sxs-lookup"><span data-stu-id="a63b1-103">Tutorial: Azure Active Directory integration with Wingspan eTMF</span></span>

<span data-ttu-id="a63b1-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Wingspan eTMF az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="a63b1-104">In this tutorial, you learn how toointegrate Wingspan eTMF with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="a63b1-105">Wingspan eTMF integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="a63b1-105">Integrating Wingspan eTMF with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="a63b1-106">Megadhatja a hozzáférés tooWingspan eTMF rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="a63b1-106">You can control in Azure AD who has access tooWingspan eTMF</span></span>
- <span data-ttu-id="a63b1-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooWingspan eTMF (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="a63b1-107">You can enable your users tooautomatically get signed-on tooWingspan eTMF (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="a63b1-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="a63b1-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="a63b1-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="a63b1-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a63b1-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="a63b1-110">Prerequisites</span></span>

<span data-ttu-id="a63b1-111">az Azure AD integrálása Wingspan eTMF tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="a63b1-111">tooconfigure Azure AD integration with Wingspan eTMF, you need hello following items:</span></span>

- <span data-ttu-id="a63b1-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="a63b1-112">An Azure AD subscription</span></span>
- <span data-ttu-id="a63b1-113">Egy Wingspan eTMF egyszeri bejelentkezés engedélyezett előfizetés</span><span class="sxs-lookup"><span data-stu-id="a63b1-113">A Wingspan eTMF single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="a63b1-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="a63b1-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="a63b1-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="a63b1-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="a63b1-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="a63b1-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="a63b1-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="a63b1-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="a63b1-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="a63b1-118">Scenario description</span></span>
<span data-ttu-id="a63b1-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="a63b1-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="a63b1-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="a63b1-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="a63b1-121">Hello gyűjteményből Wingspan eTMF hozzáadása</span><span class="sxs-lookup"><span data-stu-id="a63b1-121">Adding Wingspan eTMF from hello gallery</span></span>
2. <span data-ttu-id="a63b1-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="a63b1-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-wingspan-etmf-from-hello-gallery"></a><span data-ttu-id="a63b1-123">Hello gyűjteményből Wingspan eTMF hozzáadása</span><span class="sxs-lookup"><span data-stu-id="a63b1-123">Adding Wingspan eTMF from hello gallery</span></span>
<span data-ttu-id="a63b1-124">tooconfigure hello integrációja Wingspan eTMF az Azure AD-be, meg kell tooadd Wingspan eTMF hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="a63b1-124">tooconfigure hello integration of Wingspan eTMF into Azure AD, you need tooadd Wingspan eTMF from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="a63b1-125">**tooadd Wingspan eTMF hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="a63b1-125">**tooadd Wingspan eTMF from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="a63b1-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="a63b1-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="a63b1-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="a63b1-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="a63b1-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="a63b1-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="a63b1-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="a63b1-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="a63b1-133">Hello keresési mezőbe, írja be a **Wingspan eTMF**.</span><span class="sxs-lookup"><span data-stu-id="a63b1-133">In hello search box, type **Wingspan eTMF**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-wingspanetmf-tutorial/tutorial_wingspanetmf_search.png)

5. <span data-ttu-id="a63b1-135">A hello eredmények panelen válassza ki a **Wingspan eTMF**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="a63b1-135">In hello results panel, select **Wingspan eTMF**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-wingspanetmf-tutorial/tutorial_wingspanetmf_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="a63b1-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="a63b1-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="a63b1-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezést a Wingspan eTMF "Britta Simon." nevű tesztfelhasználó alapján</span><span class="sxs-lookup"><span data-stu-id="a63b1-138">In this section, you configure and test Azure AD single sign-on with Wingspan eTMF based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="a63b1-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Wingspan eTMF tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="a63b1-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Wingspan eTMF is tooa user in Azure AD.</span></span> <span data-ttu-id="a63b1-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Wingspan eTMF közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="a63b1-140">In other words, a link relationship between an Azure AD user and hello related user in Wingspan eTMF needs toobe established.</span></span>

<span data-ttu-id="a63b1-141">Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** a Wingspan eTMF.</span><span class="sxs-lookup"><span data-stu-id="a63b1-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Wingspan eTMF.</span></span>

<span data-ttu-id="a63b1-142">tooconfigure és az Azure AD az egyszeri bejelentkezés Wingspan eTMF-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="a63b1-142">tooconfigure and test Azure AD single sign-on with Wingspan eTMF, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="a63b1-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="a63b1-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="a63b1-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a63b1-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="a63b1-145">**[Wingspan eTMF tesztfelhasználó létrehozása](#creating-a-wingspan-etmf-test-user)**  -toohave Britta Simon egy partner, a Wingspan eTMF, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="a63b1-145">**[Creating a Wingspan eTMF test user](#creating-a-wingspan-etmf-test-user)** - toohave a counterpart of Britta Simon in Wingspan eTMF that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="a63b1-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="a63b1-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="a63b1-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="a63b1-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="a63b1-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="a63b1-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="a63b1-149">Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése, és az Wingspan eTMF alkalmazásban egyszeri bejelentkezés beállítása.</span><span class="sxs-lookup"><span data-stu-id="a63b1-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Wingspan eTMF application.</span></span>

<span data-ttu-id="a63b1-150">**tooconfigure az Azure AD egyszeri bejelentkezést a Wingspan eTMF, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="a63b1-150">**tooconfigure Azure AD single sign-on with Wingspan eTMF, perform hello following steps:**</span></span>

1. <span data-ttu-id="a63b1-151">Az Azure portál, a hello hello **Wingspan eTMF** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="a63b1-151">In hello Azure portal, on hello **Wingspan eTMF** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="a63b1-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="a63b1-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-wingspanetmf-tutorial/tutorial_wingspanetmf_samlbase.png)

3. <span data-ttu-id="a63b1-155">A hello **Wingspan eTMF tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="a63b1-155">On hello **Wingspan eTMF Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-wingspanetmf-tutorial/tutorial_wingspanetmf_url11.png)

    <span data-ttu-id="a63b1-157">a.</span><span class="sxs-lookup"><span data-stu-id="a63b1-157">a.</span></span> <span data-ttu-id="a63b1-158">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<customer name>.<instance name>.mywingspan.com/saml`</span><span class="sxs-lookup"><span data-stu-id="a63b1-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<customer name>.<instance name>.mywingspan.com/saml`</span></span>

    <span data-ttu-id="a63b1-159">b.</span><span class="sxs-lookup"><span data-stu-id="a63b1-159">b.</span></span> <span data-ttu-id="a63b1-160">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`http://saml.<instance name>.wingspan.com/shibboleth`</span><span class="sxs-lookup"><span data-stu-id="a63b1-160">In hello **Identifier** textbox, type a URL using hello following pattern: `http://saml.<instance name>.wingspan.com/shibboleth`</span></span>

    <span data-ttu-id="a63b1-161">c.</span><span class="sxs-lookup"><span data-stu-id="a63b1-161">c.</span></span> <span data-ttu-id="a63b1-162">A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<customer name>.<instance name>.mywingspan.com/`</span><span class="sxs-lookup"><span data-stu-id="a63b1-162">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<customer name>.<instance name>.mywingspan.com/`</span></span>
     
    > [!NOTE] 
    > <span data-ttu-id="a63b1-163">Ezek az értékek nincsenek hello valós.</span><span class="sxs-lookup"><span data-stu-id="a63b1-163">These values are not hello real.</span></span> <span data-ttu-id="a63b1-164">Ezek az értékek frissítése hello tényleges bejelentkezési URL-cím, a azonosítója és a válasz URL-címet is hello tényleges ügyfél neve és a példány nevét.</span><span class="sxs-lookup"><span data-stu-id="a63b1-164">Update these values with hello actual Sign-On URL, Identifier and Reply URL including hello actual customer name and instance name.</span></span> <span data-ttu-id="a63b1-165">Ügyfél [Wingspan eTMF ügyfél támogatási csoport](http://www.wingspan.com/contact-us/) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="a63b1-165">Contact [Wingspan eTMF Client support team](http://www.wingspan.com/contact-us/) tooget these values.</span></span> 

4. <span data-ttu-id="a63b1-166">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="a63b1-166">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-wingspanetmf-tutorial/tutorial_wingspanetmf_certificate.png) 

5. <span data-ttu-id="a63b1-168">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="a63b1-168">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-wingspanetmf-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="a63b1-170">tooconfigure egyszeri bejelentkezést a **Wingspan eTMF** oldalon kell letöltött toosend hello **metaadatainak XML-kódja** túl[Wingspan eTMF támogatási](http://www.wingspan.com/contact-us/).</span><span class="sxs-lookup"><span data-stu-id="a63b1-170">tooconfigure single sign-on on **Wingspan eTMF** side, you need toosend hello downloaded **Metadata XML** too[Wingspan eTMF support](http://www.wingspan.com/contact-us/).</span></span> <span data-ttu-id="a63b1-171">Ezek beállítására toohave hello SAML SSO kapcsolat mindkét oldalán megfelelően beállítva.</span><span class="sxs-lookup"><span data-stu-id="a63b1-171">They set this up toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="a63b1-172">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="a63b1-172">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="a63b1-173">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="a63b1-173">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="a63b1-174">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="a63b1-174">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="a63b1-175">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="a63b1-175">Creating an Azure AD test user</span></span>
<span data-ttu-id="a63b1-176">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="a63b1-176">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="a63b1-178">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="a63b1-178">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="a63b1-179">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="a63b1-179">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-wingspanetmf-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="a63b1-181">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="a63b1-181">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-wingspanetmf-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="a63b1-183">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="a63b1-183">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-wingspanetmf-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="a63b1-185">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="a63b1-185">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-wingspanetmf-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="a63b1-187">a.</span><span class="sxs-lookup"><span data-stu-id="a63b1-187">a.</span></span> <span data-ttu-id="a63b1-188">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="a63b1-188">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="a63b1-189">b.</span><span class="sxs-lookup"><span data-stu-id="a63b1-189">b.</span></span> <span data-ttu-id="a63b1-190">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="a63b1-190">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="a63b1-191">c.</span><span class="sxs-lookup"><span data-stu-id="a63b1-191">c.</span></span> <span data-ttu-id="a63b1-192">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="a63b1-192">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="a63b1-193">d.</span><span class="sxs-lookup"><span data-stu-id="a63b1-193">d.</span></span> <span data-ttu-id="a63b1-194">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="a63b1-194">Click **Create**.</span></span>
 
### <a name="creating-a-wingspan-etmf-test-user"></a><span data-ttu-id="a63b1-195">Wingspan eTMF tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="a63b1-195">Creating a Wingspan eTMF test user</span></span>

<span data-ttu-id="a63b1-196">Ebben a szakaszban egy Wingspan eTMF Britta Simon nevű felhasználót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="a63b1-196">In this section, you create a user called Britta Simon in Wingspan eTMF.</span></span> <span data-ttu-id="a63b1-197">Együttműködve [Wingspan eTMF támogatási](http://www.wingspan.com/contact-us/) tooadd hello felhasználók hello Wingspan eTMF alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="a63b1-197">Work with [Wingspan eTMF support](http://www.wingspan.com/contact-us/) tooadd hello users in hello Wingspan eTMF application.</span></span> <span data-ttu-id="a63b1-198">Felhasználók kell létrehoznia és aktiválni az egyszeri bejelentkezés használata előtt.</span><span class="sxs-lookup"><span data-stu-id="a63b1-198">Users must be created and activated before you use single sign-on.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="a63b1-199">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="a63b1-199">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="a63b1-200">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooWingspan eTMF megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="a63b1-200">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooWingspan eTMF.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="a63b1-202">**tooassign Britta Simon tooWingspan eTMF, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="a63b1-202">**tooassign Britta Simon tooWingspan eTMF, perform hello following steps:**</span></span>

1. <span data-ttu-id="a63b1-203">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="a63b1-203">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="a63b1-205">Hello alkalmazások listában válassza ki a **Wingspan eTMF**.</span><span class="sxs-lookup"><span data-stu-id="a63b1-205">In hello applications list, select **Wingspan eTMF**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-wingspanetmf-tutorial/tutorial_wingspanetmf_app.png) 

3. <span data-ttu-id="a63b1-207">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="a63b1-207">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="a63b1-209">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="a63b1-209">Click **Add** button.</span></span> <span data-ttu-id="a63b1-210">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="a63b1-210">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="a63b1-212">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="a63b1-212">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="a63b1-213">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="a63b1-213">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="a63b1-214">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="a63b1-214">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="a63b1-215">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="a63b1-215">Testing single sign-on</span></span>

<span data-ttu-id="a63b1-216">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="a63b1-216">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span> 

<span data-ttu-id="a63b1-217">Kattintson a hello Wingspan eTMF csempe hello hozzáférési panelre, akkor nem lesz átirányítva tooOrganization bejelentkezési lapon.</span><span class="sxs-lookup"><span data-stu-id="a63b1-217">Click hello Wingspan eTMF tile in hello Access Panel, you will be redirected tooOrganization sign on page.</span></span> <span data-ttu-id="a63b1-218">Sikeres bejelentkezés után a bejelentkezett tooyour Wingspan eTMF alkalmazás lesz.</span><span class="sxs-lookup"><span data-stu-id="a63b1-218">After successful login, you will be signed-on tooyour Wingspan eTMF application.</span></span> <span data-ttu-id="a63b1-219">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="a63b1-219">For more information about the Access Panel, see [Introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a63b1-220">További források</span><span class="sxs-lookup"><span data-stu-id="a63b1-220">Additional resources</span></span>

* [<span data-ttu-id="a63b1-221">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="a63b1-221">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="a63b1-222">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="a63b1-222">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-wingspanetmf-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-wingspanetmf-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-wingspanetmf-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-wingspanetmf-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-wingspanetmf-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-wingspanetmf-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-wingspanetmf-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-wingspanetmf-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-wingspanetmf-tutorial/tutorial_general_203.png

